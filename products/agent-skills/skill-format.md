---
description: The anatomy of a SKILL.md file — every field, every section, and how agents read them
---

# 📄 SKILL.md Format Reference

A `SKILL.md` file is the single source of truth for a skill. It tells the agent what the skill is for, what tools are available, how to use them, and what success looks like. This page documents every section of the format.

## Directory Structure

A skill package lives in its own directory. The directory name becomes the skill's identifier.

```
workspace/skills/
└── my-skill/
    ├── SKILL.md          # Required — the skill definition
    ├── helpers/          # Optional — helper scripts called by tools
    │   ├── action.js     # Node.js helper
    │   ├── process.py    # Python helper
    │   └── run.sh        # Shell helper
    └── context/          # Optional — static context files (data, templates)
        └── reference.md
```

The `SKILL.md` file is required. Everything else is optional.

## Full SKILL.md Template

```markdown
---
name: skill-name
description: One-sentence description of what this skill enables.
version: 1.0.0
author: your-name
---

# Skill Name

## Description

A longer description of what this skill does, why it exists, and when an
agent should use it. Aim for 2–4 sentences. Be precise — agents read this
to decide if the skill is relevant to the current task.

## Instructions

Detailed instructions for the agent on how to use this skill. This section
is loaded directly into the agent's context. Write it as you would write a
system prompt:

- Be explicit about what the skill can and cannot do
- List prerequisite state (e.g., "wallet must be authenticated before calling buy")
- Describe error conditions and how to handle them
- Use numbered steps for multi-step processes

## Tools

### tool_name

**Description:** What this tool does in one sentence.

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `param1` | string | Yes | The thing to act on |
| `param2` | number | No | Optional configuration value |

**Returns:** Description of the return value.

**Example call:**
```json
{
  "tool": "tool_name",
  "parameters": {
    "param1": "example-value"
  }
}
```

### another_tool

...repeat for each tool...

## Examples

### Example 1: Basic Use Case

**Prompt:** "Do the most common thing this skill handles."

**Expected behavior:**
1. Agent calls `tool_name` with the relevant parameters
2. Agent parses the response
3. Agent reports the result to the user

### Example 2: Edge Case

**Prompt:** "Handle a less common but important scenario."

**Expected behavior:**
1. Agent detects the edge condition
2. Agent calls the fallback tool
3. Agent explains what happened and why

## Error Handling

| Error | Meaning | Recommended Action |
|-------|---------|-------------------|
| `ERR_NOT_FOUND` | Resource does not exist | Inform user; do not retry |
| `ERR_RATE_LIMIT` | API rate limit hit | Wait 30s and retry once |
| `ERR_AUTH` | Authentication failed | Re-run authentication flow |
```

## Required Sections

Every `SKILL.md` must include the following sections:

### Front Matter

The YAML front matter block at the top of the file:

```yaml
---
name: skill-name           # kebab-case identifier matching the directory name
description: ...           # one-sentence summary shown in skill listings
version: 1.0.0             # semantic version
author: your-name          # creator handle or GitHub username
---
```

The `name` field must exactly match the parent directory name. Mismatches cause skill loading to fail silently.

### Description

A human-readable explanation of what the skill enables. Agents use this section to decide whether the skill applies to the current task. Be specific: "trades tokens on Solana and EVM chains using GDEX" is better than "helps with trading."

{% hint style="info" %}
The `Description` section is written for **humans** — it appears in skill listings and helps developers understand what a skill does. The `Instructions` section below is written for **agents** and is loaded directly into the agent's working context.
{% endhint %}

### Instructions

The operational guidance loaded into the agent's context. This is the most important section — it's effectively a system prompt extension. Write it precisely, because agents follow it literally.

Good instructions:
- State preconditions ("user must be authenticated")
- Define the happy path step-by-step
- Cover error handling explicitly
- Avoid ambiguity

### Tools

Each tool the skill exposes must be documented with its name, parameters, types, required status, and a JSON call example. Tool names use `snake_case`.

### Examples

At least two examples: one happy path, one edge case. Examples help agents recognize when to invoke the skill and what correct usage looks like.

## Optional Sections

### Helpers Directory

Place executable helper scripts in a `helpers/` subdirectory. Agents can invoke these via shell tool calls. Common patterns:

```
helpers/
├── buy.js          # Node.js — complex API logic, SDK calls
├── parse.py        # Python — data processing, analysis
└── setup.sh        # Shell — environment prep, checks
```

Helper scripts should be self-contained: accept inputs as CLI arguments, write results to stdout as JSON, and exit non-zero on error.

### Context Directory

Static reference files that the agent should load as background context:

```
context/
├── api-reference.md    # Endpoints, parameter docs
├── error-codes.md      # Error lookup table
└── templates/          # Reusable output templates
    └── report.md
```

The agent loads context files at skill initialization. Keep them concise — large context files consume token budget.

## Naming Conventions

| Rule | Example |
|------|---------|
| Directory and `name` field must match exactly | `gdex-spot-trading` / `name: gdex-spot-trading` |
| Use `kebab-case` for all identifiers | `my-new-skill`, not `myNewSkill` |
| Prefix GDEX-related skills with `gdex-` | `gdex-bridge`, `gdex-perp-trading` |
| Prefix payment skills with the protocol name | `tempo-payment`, `x402-payment` |
| Keep names short and descriptive | `summarize`, not `text-content-summarization-tool` |

## Best Practices

**Write instructions for the agent, not for humans.** The `Instructions` section is loaded directly into the agent's working context. Avoid prose explanations — use numbered steps, explicit conditions, and direct commands.

**Keep tools focused.** Each tool should do one thing. A `buy_token` tool and a `sell_token` tool are better than a single `trade_token` tool with a `direction` parameter.

**Document every error code.** Agents need to know what to do when things go wrong. An undocumented error code causes the agent to stall or hallucinate a response.

**Version your skills.** Increment the version in front matter when you change behavior. This helps agents and operators track which version of a skill is active.

**Test with real prompts.** For each example in your `Examples` section, verify that a real agent actually produces the described behavior. Examples that don't match real behavior erode agent trust in the skill.

---

**See also:**
- [✏️ Creating Custom Skills](creating-custom-skills.md) — step-by-step guide to building your first skill
- [🧩 Agent Skills Overview](README.md) — the full skill catalog and loading priority
