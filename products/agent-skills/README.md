---
description: Modular SKILL.md packages that teach AI agents to trade, manage portfolios, and build UIs on GDEX — installable into Claude Code, Cursor, Codex, Windsurf, and 40+ other agents
---

# 🧩 Agent Skills

{% hint style="success" %}
Agent Skills are the building blocks of the Gemach agent ecosystem. Every capability an agent has — spot trading, perpetual futures, copy trading, bridging, portfolio analytics, even generating a React trading UI — is delivered by a skill. Because skills are just markdown files, anyone can read, audit, and extend them.
{% endhint %}

The reference implementation is the open-source [`GemachDAO/gdex-skill`](https://github.com/GemachDAO/gdex-skill) package (`@gdexsdk/gdex-skill`). It ships **27 skills**, a **116-tool MCP server**, and a fully-typed **TypeScript SDK** that talk to the GDEX / Gbot trading backend.

## What Are Agent Skills?

A skill is a directory containing a `SKILL.md` file — a structured markdown document that tells an AI agent what a capability does, when to use it, and exactly which SDK calls or API endpoints to invoke. Skills are **not** compiled plugins or binaries; they are human-readable, version-controllable instructions that any agent can load and follow.

This design makes skills:

- **Auditable** — anyone can read exactly what an agent is instructed to do
- **Composable** — combine multiple skills to build multi-step workflows
- **Shareable** — published on GitHub and installed with a single command
- **Agent-native** — written for autonomous agents, not human UIs
- **Lean** — GDEX uses a **multi-skill architecture**, so an agent loads only the skills it needs and keeps its context window focused

## Three Ways to Use GDEX Skills

| Method | Best for | Entry point |
|--------|----------|-------------|
| **Skills CLI (skills.sh)** | Agents that read `SKILL.md` files directly (Claude Code, Cursor, Codex, Windsurf, +40 more) | `npx skills add GemachDAO/gdex-skill` |
| **MCP Server** | Any [Model Context Protocol](https://modelcontextprotocol.io) client — exposes 116 callable trading + docs tools | `npx @gdexsdk/mcp-server` |
| **TypeScript SDK** | Building your own agent, bot, or app in code | `npm install github:GemachDAO/gdex-skill` |

### 1. Install as Agent Skills

Install directly into any [supported agent](https://github.com/vercel-labs/skills#supported-agents) using the [skills CLI](https://skills.sh):

```bash
# Install all skills (recommended)
npx skills add GemachDAO/gdex-skill --all --agent '*' -g

# Install just the root routing skill
npx skills add GemachDAO/gdex-skill

# Install a single skill
npx skills add GemachDAO/gdex-skill --skill gdex-spot-trading
```

The root skill acts as a **router** — it tells the agent which sub-skill to load for any given task. **No API key setup is required**; shared keys are built in for read access and managed-custody trading.

### 2. Run the MCP Server

The GDEX MCP server exposes **116 tools** (108 execution + 8 documentation) so any MCP-compatible agent can trade autonomously:

```bash
# Auto-generate config for your client
npx @gdexsdk/mcp-server init --client claude   # → .mcp.json
npx @gdexsdk/mcp-server init --client cursor   # → .cursor/mcp.json
npx @gdexsdk/mcp-server init --client vscode   # → .vscode/mcp.json
npx @gdexsdk/mcp-server init --client codex    # → .codex/config.toml
npx @gdexsdk/mcp-server init --client opencode # → .opencode/mcp.json
```

### 3. Use the SDK in Code

```bash
npm install github:GemachDAO/gdex-skill
```

```typescript
import { GdexSkill, GDEX_API_KEY_PRIMARY } from '@gdexsdk/gdex-skill';

const skill = new GdexSkill();
skill.loginWithApiKey(GDEX_API_KEY_PRIMARY);   // shared key — no wallet needed

const trending = await skill.getTrendingTokens({ chain: 'solana', period: '24h', limit: 5 });
```

## Full Skill Catalog (27 skills)

Skills are grouped exactly as they are in the package's [`skills.sh.json`](https://github.com/GemachDAO/gdex-skill/blob/main/skills.sh.json) manifest.

### Getting Started

| Skill | Description |
|-------|-------------|
| `gdex-onboarding` | Start here — GDEX overview, architecture, supported chains, available skills, and quickstart |
| `gdex-authentication` | Managed-custody auth — shared API key login, session keypairs, AES-256-CBC `computedData`, secp256k1 signing |
| `gdex-wallet-setup` | Generate EVM control wallets offline, create session keypairs, fetch wallet info (no auth) |

### Trading

| Skill | Description |
|-------|-------------|
| `gdex-spot-trading` | Buy/sell tokens on Solana, Sui, and EVM chains with automatic DEX routing, slippage control, and percentage sells |
| `gdex-perp-trading` | HyperLiquid perpetual futures — open/close positions, set leverage, market/limit orders with TP/SL |
| `gdex-perp-funding` | Deposit and withdraw USDC to/from HyperLiquid for perpetual trading |
| `gdex-limit-orders` | Create, update/delete, and list limit orders via managed-custody encrypted payloads |

### Data & Discovery

| Skill | Description |
|-------|-------------|
| `gdex-portfolio` | Cross-chain portfolio overview, chain-specific balances, and paginated trade history |
| `gdex-token-discovery` | Token details, trending tokens, OHLCV candles, and top traders |
| `gdex-livestream-discovery` | Solana livestream-token discovery — live streams, per-token live status, and big-buy alert feeds |

### Copy Trading & Bridge

| Skill | Description |
|-------|-------------|
| `gdex-copy-trading` | Discover top wallets and create/manage Solana copy trades (Solana only for writes) |
| `gdex-perp-copy-trading` | Copy HyperLiquid perp traders — top traders, configs, fill history, opposite-direction copying |
| `gdex-bridge` | Cross-chain bridging of native tokens between EVM chains, Solana, and Sui via ChangeNow |

### Transfers & Social

| Skill | Description |
|-------|-------------|
| `gdex-transfers` | Native and ERC-20/SPL token transfers via managed custody to any recipient |
| `gdex-watchlist-social` | Watchlist, comments, and sentiment voting — social features around individual tokens |
| `gdex-token-import` | Import user-defined custom tokens so they appear in details, balances, and portfolio |

### HyperLiquid Extras

| Skill | Description |
|-------|-------------|
| `gdex-hl-outcomes` | HyperLiquid HIP-3 outcome / event (prediction) markets — list, order, and manage outcome positions |
| `gdex-hl-referral` | HyperLiquid referral info and reward claims |

### Promotion & Partners

| Skill | Description |
|-------|-------------|
| `gdex-trending-promotion` | Paid trending-slot promotion — book featured slots and check booking status |
| `gdex-retailer-onboarding` | Retailer partner integrations that white-label the GDEX trading stack |

### Frontend (React / Next.js)

| Skill | Description |
|-------|-------------|
| `gdex-ui-install-setup` | React/Next.js project setup — SDK init, context providers, env vars, TypeScript config |
| `gdex-ui-trading-components` | Order entry forms, position tables, copy-trade panels, orderbook and PnL views |
| `gdex-ui-portfolio-dashboard` | Portfolio dashboards — balances, trade history, chain selectors, live polling |
| `gdex-ui-wallet-connection` | Connect buttons, account displays, chain switching, API-key vs wallet auth UI |
| `gdex-ui-theming` | CSS theming — dark/light modes, trading colors, breakpoints, official Gemach brand tokens |
| `gdex-ui-page-layouts` | Full page compositions — trading, portfolio, copy-trading, and bridge pages |

### Developer Tools

| Skill | Description |
|-------|-------------|
| `gdex-sdk-debugging` | Troubleshoot SDK errors — error codes, encryption debugging, chain quirks, HL gotchas, copy-trade pitfalls |

## Supported Chains

GDEX skills operate across 12 networks through a single interface:

| Chain | ChainId | Native Token |
|-------|---------|--------------|
| Ethereum | `1` | ETH |
| Optimism | `10` | ETH |
| BNB Smart Chain | `56` | BNB |
| Sonic | `146` | S |
| Fraxtal | `252` | frxETH |
| Nibiru | `6900` | NIBI |
| Base | `8453` | ETH |
| Arbitrum One | `42161` | ETH |
| Berachain | `80094` | BERA |
| Solana | `622112261` | SOL |
| Sui | `1313131213` | SUI |
| HyperLiquid | perps only | USDC |

{% hint style="warning" %}
The Solana numeric chainId is **`622112261`** (`ChainId.SOLANA`), **not** `900`. Using `900` returns the EVM managed wallet and a `null` balance.
{% endhint %}

## Authentication Model

All trading runs through **server-side managed wallets** — your control wallet only ever signs the initial sign-in message, never the on-chain trades.

- **Shared API keys** (recommended for agents) are pre-configured in the package, so agents can read data and submit managed trades without any wallet handover.
- **Read-only endpoints** (`getTrendingTokens`, `getTokenDetails`, `getOHLCV`, `getTopTraders`) need no authentication at all.
- **Managed custody** uses a secp256k1 **session keypair** plus AES-256-CBC encrypted `computedData` payloads for write operations.

See [GDEX Trading Skills](gdex-trading-skills.md) for the full sign-in flow.

## Explore This Section

* [📄 SKILL.md Format Reference](skill-format.md) — anatomy of a skill, the `skill.json` action manifest, and packaging
* [✏️ Creating Custom Skills](creating-custom-skills.md) — add or extend a skill the gdex-skill way
* [📈 GDEX Trading Skills](gdex-trading-skills.md) — deep dive on the trading, copy-trading, and discovery skills
* [🛠️ Platform & UI Skills](utility-skills.md) — onboarding, transfers, social, HyperLiquid extras, React UI, and debugging

---

Agent Skills are open source under the MIT license. The full skill set lives in the [`GemachDAO/gdex-skill`](https://github.com/GemachDAO/gdex-skill) repository under `skills/`.
