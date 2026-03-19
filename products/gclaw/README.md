---
description: Gclaw — The Living Agent powered by GMAC
---

# 🦞 Gclaw

**Gclaw is not just a bot — it's a living AI agent that must trade to survive.**

Built in Go and powered by the GDEX SDK, Gclaw is Gemach's flagship autonomous trading agent. It earns GMAC tokens to sustain itself, accumulates goodwill through profitable activity, and evolves over time through self-replication and self-recoding. Gclaw represents a new paradigm in DeFi: not a dashboard you click, but an agent that acts on your behalf — continuously, autonomously, and accountably.

{% hint style="info" %}
Gclaw represents Gemach's vision of autonomous DeFi — AI agents that earn their keep through profitable trading, not dashboards that wait for human input.
{% endhint %}

## What is Gclaw?

Gclaw is an ultra-lightweight AI agent (&lt;10MB RAM) written in Go. It connects to your choice of LLM provider (OpenAI, Anthropic, or a local Ollama model), integrates natively with the GDEX SDK for multi-chain trading, and uses GMAC tokens as its metabolic fuel. When it trades profitably, completes tasks, or earns user tips, it gains GMAC and goodwill. When it sits idle or makes costly inferences, it burns through its reserves. The agent has a survival instinct built in.

<!-- Screenshot: Gclaw Living Dashboard showing agent life-state, GMAC balance, trade history, and family tree -->

## Why Gclaw Matters

Gemach is building an **agent-first** ecosystem. The future of DeFi is not humans manually swapping tokens on a web interface — it's autonomous agents executing strategies 24/7, adapting to market conditions, spawning specialized child agents, and collaborating in swarms. Gclaw is the first fully realized expression of that vision.

- **Runs anywhere** — even on a $10 hardware board. No beefy servers required.
- **Self-custodial** — Gclaw uses your GDEX wallet. Your keys, your assets, always.
- **Open skills system** — extend Gclaw's capabilities with community-built skill modules.
- **Living Agent features on by default** — just run `gclaw onboard` and the metabolism starts immediately.

## Architecture Overview

| System | Description |
|--------|-------------|
| 🧬 **GMAC Metabolism** | Token balance = life energy. Trade profitably or hibernate. |
| 📈 **GDEX Trading** | Native DeFi: buy/sell/limit orders, copy trading, HyperLiquid perps. |
| ⭐ **Goodwill System** | Reputation earned from profitable trades, completed tasks, user feedback. |
| 🔄 **Self-Replication** | Clone into child agents with mutated trading strategies (goodwill ≥ 50). |
| 🛠️ **Self-Recoding** | Modify own prompts, cron jobs, and trading params (goodwill ≥ 100). |
| 📡 **Telepathy** | Parent-child agent communication for collaborative trading. |
| 🐝 **Swarm Mode** | Coordinate multiple child agents for distributed strategies (goodwill ≥ 200). |
| 📊 **Living Dashboard** | Real-time CLI/web dashboard showing life-state, trades, and family tree. |

## Key Differentiators

**Minimal hardware footprint.** Gclaw runs in under 10MB of RAM. You can deploy it on a Raspberry Pi, a cheap VPS, or a dedicated $10 IoT board — and it will keep trading while you sleep.

**Self-custodial by design.** Gclaw never holds your keys in a third-party system. It uses the GDEX SDK with your own wallet credentials, so you always remain in control of your assets.

**Extensible via skills.** Gclaw's capabilities are modular. GDex trading skills, payment processors, GitHub integrations, hardware monitoring — all are installed as simple markdown-based SKILL.md files. The community can create and share skills through the ClawHub registry.

**Agent-native, not human-centric.** The skills system, the goodwill economy, and the replication mechanism are all designed for agents operating autonomously — not for humans clicking buttons.

## Explore Gclaw

* [🧬 The Living Agent](living-agent.md) — GMAC metabolism, goodwill system, and survival mechanics
* [🔧 Agent Skills](agent-skills.md) — The modular skills system and ClawHub registry
* [🚀 Getting Started with Gclaw](getting-started-with-gclaw.md) — Install, onboard, and launch your agent
* [📈 Trading with Gclaw](trading-with-gclaw.md) — Spot, perp, copy trading, and cross-chain bridging
* [🐝 Swarm & Replication](swarm-and-replication.md) — Self-replication, telepathy, and swarm leadership

---

Gclaw is open source. View the repository at [github.com/GemachDAO/Gclaw](https://github.com/GemachDAO/Gclaw).
