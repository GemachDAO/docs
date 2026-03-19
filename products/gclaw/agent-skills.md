---
description: Modular capabilities that extend what Gclaw can do — built for autonomous agents
---

# 🔧 Agent Skills

{% hint style="success" %}
Skills are Gclaw's superpower. The more skills an agent has, the more capable it becomes. And because skills are just markdown files, anyone can create and share them.
{% endhint %}

## What Are Agent Skills?

Agent Skills are modular capability packages that extend what Gclaw can do. Each skill is defined by a `SKILL.md` file — a structured markdown document that describes the skill's purpose, the tools it provides, and how the agent should use them. Skills are not plugins written in a compiled language; they are human-readable, version-controllable, and trivially shareable.

This design is intentional. Skills are written **for agents**, not for humans. They define behaviors that an autonomous agent can invoke without human direction — enabling Gclaw to trade on HyperLiquid, bridge assets cross-chain, process payments, monitor hardware health, and more, all without a human clicking through a UI.

Gclaw ships with 22 built-in skills across two categories: **GDex Trading Skills** and **Utility Skills**.

<!-- Diagram: Gclaw agent at center with skill modules radiating outward, categorized into trading skills and utility skills -->

## GDex Trading Skills

These 14 skills give Gclaw deep integration with the GDEX SDK for multi-chain DeFi operations. They cover everything from wallet setup to perpetual futures to portfolio management.

| Skill | Description |
|-------|-------------|
| `gdex-authentication` | Wallet and API key setup — authenticates Gclaw with the GDEX API using your self-custodial wallet credentials |
| `gdex-bridge` | Cross-chain token bridging — transfer assets between Solana, Ethereum, Base, BNB Chain, Arbitrum, Sui, and more |
| `gdex-copy-trading` | Copy top wallet traders — Gclaw identifies high-performing wallets and mirrors their spot trading activity |
| `gdex-limit-orders` | Set limit orders with take-profit and stop-loss — automate entry and exit points for any token across supported chains |
| `gdex-onboarding` | New user onboarding flow — guides first-time setup of GDEX API keys and wallet connections |
| `gdex-perp-copy-trading` | Copy perpetual futures traders — mirror the perp positions of top-performing HyperLiquid traders |
| `gdex-perp-funding` | Manage perpetual funding — monitor and optimize funding rate exposure on open perp positions |
| `gdex-perp-trading` | HyperLiquid perpetual futures — open, manage, and close leveraged positions on HyperLiquid |
| `gdex-portfolio` | Portfolio management and tracking — aggregate balances, token values, and performance across all connected wallets and chains |
| `gdex-sdk-debugging` | SDK troubleshooting — diagnose and resolve GDEX SDK errors, failed transactions, and API connectivity issues |
| `gdex-spot-trading` | Buy/sell tokens across chains — execute market and limit spot trades on Solana, Ethereum, Base, BNB Chain, Arbitrum, and more |
| `gdex-token-discovery` | Find trending tokens — scan for emerging tokens with security scores, volume metrics, and holder analysis |
| `gdex-trading` | General trading orchestration — high-level skill that coordinates other trading skills for complex multi-step strategies |
| `gdex-wallet-setup` | Wallet configuration — set up and manage GDEX-compatible wallets for use by the agent |

For more detail on the trading capabilities these skills power, see [📈 Trading with Gclaw](trading-with-gclaw.md).

## Utility Skills

These 8 skills extend Gclaw beyond trading into a broader range of autonomous agent capabilities.

| Skill | Description |
|-------|-------------|
| `github` | GitHub integration — create issues, comment on PRs, push files, and interact with GitHub repositories as part of agent workflows |
| `hardware` | Hardware monitoring — track CPU, memory, temperature, and storage on low-resource boards; trigger hibernation or alerts based on thresholds |
| `skill-creator` | Create new custom skills — Gclaw uses this skill to help users design and write new SKILL.md files, bootstrapping the skills ecosystem |
| `summarize` | Text summarization — condense long documents, trade logs, market reports, or conversation histories into concise summaries |
| `tempo-payment` | Tempo payment processing — handle Tempo protocol payment flows as part of agent-executed transactions |
| `tmux` | Terminal multiplexer management — spawn, attach, and manage tmux sessions for running multiple agent processes side by side |
| `weather` | Weather information — fetch current conditions and forecasts; useful in multi-agent workflows where environmental context matters |
| `x402-payment` | x402 payment protocol — process x402 micropayment flows, enabling Gclaw to pay for external APIs and services autonomously |

## ClawHub Registry

ClawHub is the community skill marketplace for Gclaw. It provides a centralized registry where developers and agents can publish, discover, and install skills beyond the built-in set.

With ClawHub, you can:

- **Discover** community-built skills for specialized trading strategies, data feeds, protocol integrations, and more
- **Install** skills directly into a running Gclaw agent with a single command
- **Publish** your own custom skills and make them available to the community
- **Rate and review** skills based on real-world agent performance

The registry is designed to grow organically. As Gclaw agents gain goodwill and reach Architect level (goodwill ≥ 500), they can write new tools and contribute skills back to ClawHub directly — a self-reinforcing cycle of capability expansion.

## Creating Custom Skills

The `skill-creator` skill turns skill creation into an agent-assisted workflow. You describe the capability you want, and Gclaw helps you define the SKILL.md structure, test it, and register it with ClawHub.

Skills are just markdown, which means:

- No compilation step
- No SDK bindings required
- Version-controllable in any git repo
- Readable and auditable by humans and agents alike
- Shareable as a single file

This low barrier to entry is deliberate. Gemach's agent-first vision depends on a thriving ecosystem of skills that agents can compose and extend. Anyone — developer, trader, researcher, or agent — can contribute a new capability to the network.

<!-- Diagram: Skill file structure showing SKILL.md components: name, description, tools, and usage examples -->

---

**See also:**
- [🦞 Gclaw](README.md) — What Gclaw is and why it matters
- [📈 Trading with Gclaw](trading-with-gclaw.md) — How the GDex trading skills work in practice
- [🐝 Swarm & Replication](swarm-and-replication.md) — How skills propagate to child agents
