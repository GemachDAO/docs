---
description: The anatomy of a GDEX SKILL.md file, the skill.json action manifest, and how the gdex-skill package is structured
---

# 📄 SKILL.md Format Reference

A `SKILL.md` file is the single source of truth for a skill. It tells the agent what the skill is for, when to use it, and exactly which SDK methods or API endpoints to call. This page documents the format used by the [`GemachDAO/gdex-skill`](https://github.com/GemachDAO/gdex-skill) package.

## Package Layout

The gdex-skill repository follows a **multi-skill** layout: each skill is its own directory under `skills/`, and a top-level `skill.json` describes the machine-readable action manifest consumed by [skills.sh](https://skills.sh) and the MCP server.

```
gdex-skill/
├── skill.json              # skills.sh action manifest (name, auth, base_url, actions[])
├── skills.sh.json          # grouping/ordering of skills for the skills.sh CLI
├── skills/
│   ├── gdex-onboarding/
│   │   └── SKILL.md        # one skill = one directory + one SKILL.md
│   ├── gdex-spot-trading/
│   │   └── SKILL.md
│   └── …                   # 27 skills total
├── src/                    # the @gdexsdk/gdex-skill TypeScript SDK
└── mcp-server/             # the 116-tool MCP server
```

The directory name becomes the skill's identifier (e.g. `gdex-spot-trading`) and must match the `name` field in the `SKILL.md` front matter.

## SKILL.md Front Matter

Every `SKILL.md` opens with a minimal YAML front-matter block — just two fields:

```yaml
---
name: gdex-spot-trading
description: Buy and sell tokens on Solana, Sui, and EVM chains with automatic DEX routing, slippage control, and percentage-based sells
---
```

| Field | Required | Purpose |
|-------|----------|---------|
| `name` | ✅ | kebab-case identifier; must match the parent directory name exactly |
| `description` | ✅ | One sentence shown in skill listings; agents read this to decide whether the skill is relevant |

{% hint style="info" %}
The `description` is the most important line in the file. The agent uses it to route a user request to the correct skill, so it should clearly state *what* the skill does and *when* to reach for it.
{% endhint %}

## SKILL.md Body

After the front matter, the body is plain markdown written **for the agent**. GDEX skills follow a consistent structure:

````markdown
# GDEX: Spot Trading

Buy and sell tokens across Solana, Sui, and 12+ EVM chains. The SDK routes
through the best available DEX automatically, or you can specify one.

## When to Use

- Buying a token with native currency (SOL, ETH, etc.)
- Selling a token (absolute amount or percentage of holdings)
- Executing managed-custody spot trades with encrypted payloads

## Prerequisites

- `@gdexsdk/gdex-skill` installed
- Authenticated via `loginWithApiKey()` or managed-custody sign-in
- See **gdex-authentication** for auth setup

## Buy a Token

```typescript
const trade = await skill.buyToken({
  chain: 'solana',
  tokenAddress: 'EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v',
  amount: '0.1',     // 0.1 SOL
  slippage: 1,       // 1% max slippage
});
// Returns: { jobId, status, inputAmount, outputAmount, txHash?, error? }
```

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `chain` | string \| ChainId | Yes | `'solana'`, `'sui'`, or a ChainId number |
| `tokenAddress` | string | Yes | Contract address of the token |
| `amount` | string | Yes | Native token input amount |
| `slippage` | number | No | Max slippage % (default: 1) |

> **Critical:** notes that prevent common mistakes go in blockquotes.
````

### Recommended sections

| Section | Purpose |
|---------|---------|
| **Title + intro** | One-line `# GDEX: <Name>` heading plus a short paragraph |
| **When to Use** | Bullet list of the scenarios that should trigger the skill |
| **Prerequisites** | Required install/auth state and cross-references to sibling skills |
| **Method sections** | One per operation, each with a TypeScript example and a parameter table |
| **Warnings / Critical notes** | Blockquotes that flag footguns (e.g. the Solana chainId, deterministic AES, delete-vs-toggle behavior) |

Skills cross-reference each other by **name** in bold (e.g. *see **gdex-authentication***) so the agent knows which sibling skill to load next.

## The `skill.json` Action Manifest

Alongside the human-readable `SKILL.md` files, the package ships a machine-readable `skill.json` that the skills.sh CLI and MCP server use to expose callable actions. Each action declares a JSON-Schema parameter spec:

```json
{
  "name": "@gdexsdk/gdex-skill",
  "version": "1.0.0",
  "description": "Cross-chain DeFi trading skill for AI agents…",
  "homepage": "https://github.com/GemachDAO/gdex-skill",
  "categories": ["finance", "trading", "defi", "crypto"],
  "auth": {
    "type": "computed_data",
    "encryption": "AES-256-CBC",
    "session_signing": "secp256k1"
  },
  "base_url": "https://trade-api.gemach.io/v1",
  "actions": [
    {
      "name": "buy_token",
      "description": "Buy a token on any supported chain using the best DEX route.",
      "parameters": {
        "type": "object",
        "required": ["chain", "tokenAddress", "amount"],
        "properties": {
          "chain": { "type": ["string", "number"], "description": "Chain name or ChainId" },
          "tokenAddress": { "type": "string", "description": "Token contract address" },
          "amount": { "type": "string", "description": "Native token input amount" },
          "slippage": { "type": "number", "description": "Max slippage % (default 1)" }
        }
      }
    }
  ]
}
```

## Skill Grouping — `skills.sh.json`

The `skills.sh.json` file controls how the skills are grouped and ordered when an agent browses or installs them:

```json
{
  "$schema": "https://skills.sh/schemas/skills.sh.schema.json",
  "groupings": [
    {
      "title": "Trading",
      "description": "Spot swaps, HyperLiquid perpetual futures, funding, and limit orders.",
      "skills": ["gdex-spot-trading", "gdex-perp-trading", "gdex-perp-funding", "gdex-limit-orders"]
    }
  ]
}
```

## Naming Conventions

| Rule | Example |
|------|---------|
| Directory and `name` field must match exactly | `gdex-spot-trading` / `name: gdex-spot-trading` |
| Use `kebab-case` for all identifiers | `gdex-limit-orders`, not `gdexLimitOrders` |
| Prefix GDEX skills with `gdex-` | `gdex-bridge`, `gdex-perp-trading` |
| Prefix UI skills with `gdex-ui-` | `gdex-ui-trading-components` |
| Keep names short and descriptive | `gdex-portfolio`, not `gdex-cross-chain-portfolio-balance-viewer` |

## Best Practices

**Write for the agent, not for humans.** The body is loaded directly into the agent's working context. Prefer concrete TypeScript examples and parameter tables over prose.

**Show the exact method signature.** Every method section should include a runnable `skill.<method>()` example and the shape of the returned object.

**Flag the footguns.** Use blockquotes for the non-obvious constraints that cause silent failures — the Solana chainId (`622112261`), deterministic AES (no random IV), and the fact that copy-trade `isDelete`/`isChangeStatus` both permanently delete.

**Keep one skill focused.** A `buy_token` and a `sell_token` action are better than a single `trade` action with a `direction` flag — this is why GDEX splits capability across 27 small skills.

---

**See also:**
- [✏️ Creating Custom Skills](creating-custom-skills.md) — build or extend a skill step by step
- [🧩 Agent Skills Overview](README.md) — the full catalog and install options
