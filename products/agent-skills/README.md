---
description: Modular instruction sets that teach AI agents new capabilities — works with any agent in the Gemach ecosystem
---

# 🧩 Agent Skills

{% hint style="success" %}
Agent Skills are the building blocks of the Gemach agent ecosystem. Every capability an agent has — trading on-chain, bridging assets, managing payments, monitoring hardware — is delivered by a skill. And because skills are just markdown files, anyone can read, write, and share them.
{% endhint %}

## What Are Agent Skills?

Agent Skills are modular capability packages defined by `SKILL.md` files. A `SKILL.md` is a structured markdown document that describes a discrete capability: what the skill does, what tools it exposes, how the agent should invoke those tools, and example prompts that exercise it.

Skills are **not** compiled plugins or binary extensions. They are human-readable, version-controllable markdown files that any AI agent can load and follow. This design makes skills:

- **Auditable** — anyone can read exactly what an agent is instructed to do
- **Composable** — combine multiple skills to build complex, multi-step workflows
- **Shareable** — drop a `SKILL.md` file into any git repo and it's instantly available
- **Agent-native** — written for autonomous agents, not human UIs

Gemach ships **22 built-in skills** organized into two categories: GDEX Trading Skills and Utility Skills. These are available in every agent out of the box.

## How Skills Are Loaded

Skills follow a **three-tier loading priority**:

```
workspace skills  (highest priority)
       ↓
global skills
       ↓
built-in skills   (lowest priority / fallback)
```

| Tier | Location | Purpose |
|------|----------|---------|
| **Workspace** | `./workspace/skills/<skill-name>/SKILL.md` | Project-specific overrides — highest priority |
| **Global** | `~/.gclaw/skills/<skill-name>/SKILL.md` | User-level skills shared across all agents |
| **Built-in** | Bundled with the agent binary | Default skills that ship with every install |

If a skill with the same name exists at multiple tiers, the highest-priority version wins. This lets you override a built-in skill's behavior for a specific project without modifying global configuration.

## Full Skill Catalog

### GDEX Trading Skills (14)

These skills give agents deep integration with the GDEX SDK for multi-chain DeFi operations.

| Skill | Directory | Description |
|-------|-----------|-------------|
| Spot Trading | `gdex-spot-trading` | Buy and sell tokens on Solana, Ethereum, Base, BNB Chain, Arbitrum, and more |
| Perpetual Trading | `gdex-perp-trading` | Open, manage, and close leveraged long/short positions on HyperLiquid |
| Copy Trading | `gdex-copy-trading` | Mirror the spot trades of top-performing wallets automatically |
| Perp Copy Trading | `gdex-perp-copy-trading` | Copy perpetual futures strategies from high-performing HyperLiquid traders |
| Perp Funding | `gdex-perp-funding` | Monitor and manage perpetual funding rate exposure on open positions |
| Limit Orders | `gdex-limit-orders` | Place limit buy/sell orders with take-profit and stop-loss automation |
| Bridge | `gdex-bridge` | Transfer assets cross-chain between all supported GDEX networks |
| Token Discovery | `gdex-token-discovery` | Scan for trending and newly launched tokens with security scoring |
| Portfolio | `gdex-portfolio` | Track holdings, balances, and P&L across all wallets and chains |
| Authentication | `gdex-authentication` | Manage GDEX session key pair auth — sign-in messages, computed data |
| Wallet Setup | `gdex-wallet-setup` | Configure and manage GDEX-compatible wallets for agent use |
| Onboarding | `gdex-onboarding` | First-time GDEX setup wizard for API keys and wallet connections |
| SDK Debugging | `gdex-sdk-debugging` | Diagnose and fix GDEX SDK errors, failed transactions, API issues |
| Trading (General) | `gdex-trading` | High-level orchestration skill that coordinates other trading skills |

### Utility Skills (8)

These skills extend agents beyond trading into a broader range of autonomous capabilities.

| Skill | Directory | Description |
|-------|-----------|-------------|
| Skill Creator | `skill-creator` | Create new custom skills — describe a capability and the agent scaffolds the SKILL.md |
| GitHub | `github` | Create issues, comment on PRs, push files, and manage repositories |
| Summarize | `summarize` | Condense documents, trade logs, market reports, or conversations |
| Hardware | `hardware` | Monitor CPU, memory, temperature, and storage; trigger alerts on thresholds |
| Weather | `weather` | Fetch current conditions and forecasts for use in multi-agent workflows |
| Tmux | `tmux` | Spawn and manage tmux sessions for running multiple agent processes |
| Tempo Payment | `tempo-payment` | Handle Tempo protocol payment flows in agent-executed transactions |
| x402 Payment | `x402-payment` | Process x402 micropayment flows so agents can pay for external APIs autonomously |

## Supported Chains

GDEX trading skills operate across the following networks:

| Chain | Chain ID |
|-------|----------|
| Solana | 622112261 |
| Ethereum | 1 |
| Base | 8453 |
| Arbitrum | 42161 |
| BNB Smart Chain | 56 |
| Optimism | 10 |
| Sui | (native) |
| Sonic | (native) |
| Berachain | (native) |

## ClawHub — Community Skill Registry

ClawHub is the community marketplace for agent skills. Developers and agents can publish, discover, and install skills beyond the 22 built-ins.

- **Discover** community-built skills for specialized strategies, data feeds, and protocol integrations
- **Install** skills into a running agent with a single command
- **Publish** your own skills and make them available to the ecosystem
- **Rate** skills based on real-world agent performance

## Explore This Section

* [📄 SKILL.md Format Reference](skill-format.md) — anatomy of a skill file, required sections, best practices
* [✏️ Creating Custom Skills](creating-custom-skills.md) — step-by-step guide to building your first skill
* [📈 GDEX Trading Skills](gdex-trading-skills.md) — deep dive on all 14 GDEX trading skills
* [🛠️ Utility Skills](utility-skills.md) — complete reference for all 8 utility skills

---

Agent Skills are open source. The built-in skill set lives in the [`GemachDAO/Gclaw`](https://github.com/GemachDAO/Gclaw) repository under `workspace/skills/`.
