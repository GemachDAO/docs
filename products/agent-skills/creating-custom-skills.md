---
description: Build your own SKILL.md from scratch — templates, naming rules, helper scripts, testing, and publishing to ClawHub
---

# ✏️ Creating Custom Skills

Custom skills let you extend any Gemach agent with new capabilities specific to your workflow, your APIs, or your trading strategy. This guide walks you through the entire process — from idea to ClawHub-published skill.

## Prerequisites

- A working Gclaw installation (see [Getting Started with Gclaw](../gclaw/getting-started-with-gclaw.md))
- Basic familiarity with the [SKILL.md format](skill-format.md)
- Node.js, Python, or shell scripting knowledge (for helper scripts)

## Option A: Use the `skill-creator` Meta-Skill

The fastest way to build a new skill is to let the agent help you scaffold it. Gclaw ships with a built-in `skill-creator` skill that turns natural language descriptions into SKILL.md boilerplate.

**Step 1.** Launch Gclaw and describe what you want:

```
You: Create a new skill that fetches the current price of any token from
     the CoinGecko API and returns the price in USD.
```

**Step 2.** The agent generates a scaffold:

```
Gclaw: I've created a new skill at workspace/skills/coingecko-price/SKILL.md.
       It includes a get_token_price tool and a helpers/fetch-price.js script.
       Review the files and let me know what to adjust.
```

**Step 3.** Review and refine. The scaffold gives you the structure — you fill in the details that only you know (API keys, endpoint specifics, error handling for your use case).

## Option B: Build from Scratch

### Step 1 — Create the Directory

Skill directory names must be `kebab-case` and must match the `name` field in the front matter exactly:

```bash
mkdir -p workspace/skills/my-skill
cd workspace/skills/my-skill
```

### Step 2 — Write the SKILL.md

Start with the template from the [SKILL.md Format Reference](skill-format.md#full-skillmd-template) and fill in each section.

Here is a minimal but complete working example — a skill that checks the health of an external API endpoint:

```markdown
---
name: api-health-check
description: Check whether an external HTTP endpoint is reachable and returning expected responses.
version: 1.0.0
author: your-handle
---

# API Health Check

## Description

Checks the availability and response time of an external HTTP endpoint.
Useful in automated workflows where downstream actions depend on an API
being healthy before proceeding.

## Instructions

Use this skill when you need to verify that an external service is online
before making dependent calls.

1. Call `check_endpoint` with the target URL.
2. Parse the `status`, `latency_ms`, and `body_preview` fields from the response.
3. If `status` is not 200, report the failure to the user and do not proceed
   with downstream actions.
4. If `latency_ms` exceeds 3000, warn the user that the API is slow.

## Tools

### check_endpoint

**Description:** Sends a GET request to the provided URL and returns status, latency, and body preview.

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `url` | string | Yes | The full URL to check (e.g. `https://api.example.com/health`) |
| `timeout_ms` | number | No | Request timeout in milliseconds. Default: 5000 |

**Returns:** `{ status: number, latency_ms: number, body_preview: string }`

**Example call:**
```json
{
  "tool": "check_endpoint",
  "parameters": {
    "url": "https://api.example.com/health",
    "timeout_ms": 3000
  }
}
```

## Examples

### Example 1: Healthy endpoint

**Prompt:** "Is the Gemach API online?"

**Expected behavior:**
1. Agent calls `check_endpoint` with `https://api.gemach.io/health`
2. Receives `{ status: 200, latency_ms: 142, body_preview: "{\"ok\":true}" }`
3. Reports: "Gemach API is online. Response time: 142ms."

### Example 2: Unreachable endpoint

**Prompt:** "Check if my local server at localhost:3000 is running."

**Expected behavior:**
1. Agent calls `check_endpoint` with `http://localhost:3000`
2. Receives an error (connection refused)
3. Reports: "localhost:3000 is not reachable. Check that your server is running."

## Error Handling

| Error | Meaning | Recommended Action |
|-------|---------|-------------------|
| `ERR_TIMEOUT` | Request exceeded timeout | Retry once with doubled timeout |
| `ERR_CONNECTION` | Host unreachable | Report failure; do not retry automatically |
| `ERR_SSL` | Certificate error | Report the SSL issue specifically |
```

### Step 3 — Add Helper Scripts

If your skill needs to make API calls, process data, or run complex logic, add helper scripts to a `helpers/` subdirectory.

**Node.js helper example** (`helpers/check-endpoint.js`):

```javascript
#!/usr/bin/env node
/**
 * Usage: node check-endpoint.js <url> [timeout_ms]
 * Output: JSON to stdout
 */

const https = require('https');
const http = require('http');
const { URL } = require('url');

const url = process.argv[2];
const timeoutMs = parseInt(process.argv[3] || '5000', 10);

if (!url) {
  console.error(JSON.stringify({ error: 'ERR_NO_URL', message: 'URL argument required' }));
  process.exit(1);
}

const start = Date.now();
const parsedUrl = new URL(url);
const client = parsedUrl.protocol === 'https:' ? https : http;

const req = client.get(url, { timeout: timeoutMs }, (res) => {
  let body = '';
  res.on('data', (chunk) => { body += chunk; });
  res.on('end', () => {
    console.log(JSON.stringify({
      status: res.statusCode,
      latency_ms: Date.now() - start,
      body_preview: body.slice(0, 200),
    }));
  });
});

req.on('timeout', () => {
  req.destroy();
  console.error(JSON.stringify({ error: 'ERR_TIMEOUT', message: `Request timed out after ${timeoutMs}ms` }));
  process.exit(1);
});

req.on('error', (err) => {
  console.error(JSON.stringify({ error: 'ERR_CONNECTION', message: err.message }));
  process.exit(1);
});
```

**Python helper example** (`helpers/parse-response.py`):

```python
#!/usr/bin/env python3
"""
Usage: python3 parse-response.py '<json_string>'
Output: processed JSON to stdout
"""

import json
import sys

def parse(raw: str) -> dict:
    data = json.loads(raw)
    return {
        "healthy": data.get("status") == 200,
        "fast": data.get("latency_ms", 9999) < 1000,
        "summary": f"HTTP {data.get('status')} in {data.get('latency_ms')}ms",
    }

if __name__ == "__main__":
    try:
        result = parse(sys.argv[1])
        print(json.dumps(result))
    except json.JSONDecodeError as e:
        print(json.dumps({"error": "ERR_PARSE_FAILED", "message": str(e)}))
        sys.exit(1)
    except Exception as e:
        print(json.dumps({"error": "ERR_UNKNOWN", "message": str(e)}))
        sys.exit(1)
```

### Step 4 — Test Locally

Testing a skill is straightforward: load it into a running agent and fire prompts that exercise each example in your `Examples` section.

```bash
# Launch Gclaw and load your workspace skill
gclaw chat

# In the chat interface, test with your example prompts
> Is api.example.com reachable?
> Check my localhost:3000 server
> What happens if I pass a bad URL?
```

Checklist for a skill that passes testing:

- [ ] Every example prompt produces the behavior described in `Examples`
- [ ] Error conditions are handled gracefully (no unhandled panics or hallucinated responses)
- [ ] The skill does not interfere with other loaded skills
- [ ] Helper scripts exit non-zero on error and write valid JSON to stdout

### Step 5 — Validate the SKILL.md

Before publishing, verify your file structure:

```
my-skill/
├── SKILL.md             ✅ Present
├── helpers/
│   └── check-endpoint.js  ✅ Executable, accepts CLI args, outputs JSON
└── context/             (optional)
```

Run a quick lint check on the front matter:

```bash
# Verify name matches directory
grep "^name:" workspace/skills/my-skill/SKILL.md
# Should output: name: my-skill
```

## Naming Conventions

| Convention | Correct | Incorrect |
|------------|---------|-----------|
| kebab-case identifiers | `price-tracker` | `PriceTracker`, `price_tracker` |
| GDEX skills prefixed with `gdex-` | `gdex-my-strategy` | `my-gdex-strategy` |
| Payment skills prefixed with protocol | `tempo-payment` | `payment-tempo` |
| Concise, action-oriented names | `summarize` | `text-document-summarization-utility` |
| Directory name matches `name` field | both = `my-skill` | directory = `my_skill`, name = `my-skill` |

## Publishing to ClawHub

Once your skill is tested and working, you can share it with the Gemach community through ClawHub — the community skill registry for Gemach agents.

**1. Prepare your skill package:**
```
my-skill/
├── SKILL.md
├── README.md      # Optional but recommended — human-readable docs
├── helpers/
└── LICENSE        # MIT recommended
```

**2. Push to a public GitHub repository:**
```bash
git init
git add .
git commit -m "feat: initial skill release"
git remote add origin https://github.com/your-handle/gclaw-skill-my-skill
git push -u origin main
```

**3. Submit to ClawHub** by opening a PR to the [GemachDAO/clawhub](https://github.com/GemachDAO/clawhub) registry repository. Include:
- Your skill's GitHub repository URL
- A short description of what the skill does
- The category tag (`gdex-trading`, `utility`, `payments`, etc.)

Once merged, agents can install your skill directly:

```bash
gclaw skills install your-handle/my-skill
```

---

**See also:**
- [📄 SKILL.md Format Reference](skill-format.md) — every field and section documented
- [🧩 Agent Skills Overview](README.md) — how skills are loaded and prioritized
- [📈 GDEX Trading Skills](gdex-trading-skills.md) — study the built-in skills as examples
