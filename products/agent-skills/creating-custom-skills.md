---
description: Add or extend a GDEX skill — write a SKILL.md, register an action in skill.json, validate, and install it into your agent
---

# ✏️ Creating Custom Skills

Custom skills let you extend the GDEX agent stack with capabilities specific to your workflow, your strategy, or a new GDEX endpoint. This guide follows the conventions used by the open-source [`GemachDAO/gdex-skill`](https://github.com/GemachDAO/gdex-skill) package.

## Prerequisites

- Node.js 18+ (the SDK and MCP server target `>=18`)
- Familiarity with the [SKILL.md format](skill-format.md)
- A clone of the skill repository (or your own fork):

```bash
git clone https://github.com/GemachDAO/gdex-skill
cd gdex-skill
npm install
```

## Step 1 — Create the Skill Directory

Skill directories live under `skills/` and use `kebab-case` names. The directory name must match the `name` field in the front matter exactly:

```bash
mkdir -p skills/gdex-my-strategy
```

## Step 2 — Write the SKILL.md

Create `skills/gdex-my-strategy/SKILL.md`. Keep the front matter to `name` + `description`, then write the body for the agent — a short intro, a **When to Use** list, **Prerequisites**, and one section per operation with a runnable example and a parameter table.

````markdown
---
name: gdex-my-strategy
description: Run a momentum strategy — discover trending Solana tokens, buy the top movers, and set take-profit limit sells
---

# GDEX: My Strategy

Discovers trending tokens, buys the strongest movers, and arms a take-profit
limit sell on each fill.

## When to Use

- The user asks to "run my momentum strategy" or to auto-buy trending tokens
- A scheduled job needs to rebalance into the day's top movers

## Prerequisites

- `@gdexsdk/gdex-skill` installed
- Authenticated via `loginWithApiKey()` (see **gdex-authentication**)
- Familiarity with **gdex-token-discovery**, **gdex-spot-trading**, and
  **gdex-limit-orders**

## Run the Strategy

```typescript
import { GdexSkill, GDEX_API_KEY_PRIMARY } from '@gdexsdk/gdex-skill';

const skill = new GdexSkill();
skill.loginWithApiKey(GDEX_API_KEY_PRIMARY);

const trending = await skill.getTrendingTokens({ chain: 'solana', period: '6h', limit: 3 });

for (const token of trending) {
  await skill.buyToken({ chain: 'solana', tokenAddress: token.address, amount: '0.05' });
  // then arm a take-profit via gdex-limit-orders
}
```

> **Critical:** the Solana chainId is `622112261`, not `900`.
````

{% hint style="info" %}
Reuse existing skills wherever possible. A new strategy skill should **compose** `gdex-token-discovery`, `gdex-spot-trading`, and `gdex-limit-orders` rather than re-implement trading logic.
{% endhint %}

## Step 3 — Register Actions (optional)

If your skill exposes a new callable action to the skills.sh CLI or MCP server, add it to the top-level `skill.json` `actions` array with a JSON-Schema parameter spec:

```json
{
  "name": "run_my_strategy",
  "description": "Discover trending tokens and buy the top movers with a take-profit.",
  "parameters": {
    "type": "object",
    "required": ["chain"],
    "properties": {
      "chain": { "type": ["string", "number"], "description": "Chain name or ChainId" },
      "limit": { "type": "number", "description": "How many tokens to buy (default 3)" }
    }
  }
}
```

Then add the skill name to the appropriate group in `skills.sh.json` so it shows up in the CLI.

## Step 4 — Validate

The repository ships scripts to check skills and the SDK without touching the network:

```bash
npm run validate        # validate skill definitions
npm run lint            # eslint over src and tests
npm test                # jest suite (mocked HTTP — no API key or network)
npm run verify          # offline SDK smoke test (no network token required)
npm run verify:managed  # dry-run the managed-custody payload pipeline
```

## Step 5 — Install Into Your Agent

Install your fork's skills with the skills CLI:

```bash
# All skills from your fork
npx skills add your-handle/gdex-skill --all --agent '*' -g

# Just your new skill
npx skills add your-handle/gdex-skill --skill gdex-my-strategy
```

Or expose it through the MCP server so any MCP client can call it:

```bash
npx @gdexsdk/mcp-server init --client claude
```

## Naming Conventions

| Convention | Correct | Incorrect |
|------------|---------|-----------|
| kebab-case identifiers | `gdex-my-strategy` | `gdexMyStrategy`, `gdex_my_strategy` |
| GDEX skills prefixed with `gdex-` | `gdex-my-strategy` | `my-gdex-strategy` |
| UI skills prefixed with `gdex-ui-` | `gdex-ui-my-widget` | `gdex-my-widget-ui` |
| Concise, action-oriented names | `gdex-portfolio` | `gdex-cross-chain-portfolio-viewer` |
| Directory name matches `name` field | both = `gdex-my-strategy` | dir = `gdex_my_strategy`, name = `gdex-my-strategy` |

## Contributing Upstream

To contribute a skill back to the ecosystem, open a pull request against [`GemachDAO/gdex-skill`](https://github.com/GemachDAO/gdex-skill) with:

- the new `skills/<name>/SKILL.md`
- any `skill.json` / `skills.sh.json` additions
- passing `npm run validate`, `npm run lint`, and `npm test`

See the repository's `CONTRIBUTING` notes and `SECURITY.md` before submitting.

---

**See also:**
- [📄 SKILL.md Format Reference](skill-format.md) — every field and section documented
- [🧩 Agent Skills Overview](README.md) — install options and the full catalog
- [📈 GDEX Trading Skills](gdex-trading-skills.md) — study the built-in skills as examples
