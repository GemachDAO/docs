---
description: Install Gclaw, run onboarding, and launch your first living AI trading agent
---

# 🚀 Getting Started with Gclaw

{% hint style="info" %}
Living Agent features are enabled out of the box — no extra configuration needed. Run `gclaw onboard`, follow the prompts, and your agent starts its metabolism immediately.
{% endhint %}

This guide walks you through everything you need to get Gclaw running: installing the binary, completing onboarding, launching the agent, and verifying it's alive via the Living Dashboard.

## Prerequisites

Before you begin, make sure you have:

- **Go 1.21 or later** (if installing from source) — download at [go.dev/dl](https://go.dev/dl)
- **An LLM API key** — Gclaw supports [OpenAI](https://platform.openai.com), [Anthropic](https://anthropic.com), and local [Ollama](https://ollama.ai) models
- **A GDEX API key** _(optional but recommended)_ — enables live trading via [GDex Pro](../gdex-pro/README.md); without it, Gclaw runs in market-monitor mode only
- **50 MB of free disk space** — the Gclaw binary is under 10 MB; the rest is config and agent memory

Gclaw is designed to run on minimal hardware. It will work on a Raspberry Pi, a $10 IoT board, a cheap VPS, or your laptop.

<!-- Screenshot: Terminal showing Go version check and Gclaw binary size after installation -->

## Step 1: Install Gclaw

**Option A — Install with Go:**

```bash
go install github.com/GemachDAO/Gclaw/cmd/gclaw@latest
```

**Option B — Download a pre-built binary:**

Download the latest release for your platform from the [Gclaw releases page](https://github.com/GemachDAO/Gclaw/releases), then move it to your `PATH`:

```bash
# Example for Linux/macOS (amd64)
chmod +x gclaw
mv gclaw /usr/local/bin/gclaw
```

Verify the installation:

```bash
gclaw version
```

## Step 2: Run Onboarding

Onboarding configures your LLM provider, optional GDEX trading credentials, and initial agent parameters.

```bash
gclaw onboard
```

The interactive onboarding flow will ask you to:

1. **Choose your LLM provider** — OpenAI, Anthropic, or Ollama (local)
2. **Enter your API key** — stored locally in `~/.gclaw/config.json`
3. **Configure GDEX trading** _(optional)_ — enter your GDEX API key and wallet address
4. **Set your risk preferences** — initial position size, maximum drawdown, and preferred chains

<!-- Screenshot: Terminal showing the gclaw onboard interactive prompt with LLM provider selection -->

## Step 3: Start the Agent

Once onboarding is complete, launch your agent:

```bash
gclaw agent
```

This starts the Living Agent process. You will see the agent's startup sequence in the terminal — including its initial GMAC balance, the LLM provider it connected to, and confirmation that the metabolism has started ticking.

## What Happens Automatically

As soon as the agent starts, several systems activate without any additional setup:

- **GMAC metabolism begins** — the agent starts at 1,000 GMAC and begins burning a small amount per heartbeat tick
- **Market scanning starts** — Gclaw immediately begins monitoring token prices and trends via GDEX
- **GDEX trading is enabled** _(if credentials were provided)_ — the agent evaluates trade opportunities based on its default strategy
- **Skills load** — all built-in skills (trading, portfolio management, token discovery, etc.) are available immediately
- **Living Dashboard launches** — accessible at `http://127.0.0.1:18790`

## The Living Dashboard

Open your browser and navigate to:

```
http://127.0.0.1:18790
```

The dashboard gives you a real-time view of your agent's life state:

- **GMAC balance** — current token reserves and burn rate
- **Goodwill score** — reputation progress toward the next ability threshold
- **Active trades** — open positions and recent trade history
- **Heartbeat status** — tick frequency and inference cost log
- **Family tree** — child agents (if any have been spawned via self-replication)

<!-- Screenshot: Living Dashboard browser view with GMAC gauge, goodwill progress bar, and trade history table -->

## Configuration

Gclaw stores its configuration at `~/.gclaw/config.json`. You can edit this file directly to customize metabolism settings, trading parameters, and agent behavior.

Example metabolism configuration:

```json
{
  "metabolism": {
    "initial_gmac": 1000,
    "heartbeat_cost": 0.1,
    "inference_cost_per_1k_tokens": 0.5,
    "survival_threshold": 50
  },
  "trading": {
    "enabled": true,
    "max_position_size_usd": 50,
    "preferred_chains": ["solana", "base", "arbitrum"],
    "stop_loss_percent": 5,
    "take_profit_percent": 15
  },
  "llm": {
    "provider": "openai",
    "model": "gpt-4o-mini"
  },
  "dashboard": {
    "port": 18790,
    "enabled": true
  }
}
```

Changes to the config file take effect on the next agent restart. Agents with goodwill ≥ 100 can also self-modify some of these parameters autonomously — see [🧬 The Living Agent](living-agent.md) for details.

## Next Steps

Your agent is live. Here's what to explore next:

- **[🧬 The Living Agent](living-agent.md)** — Understand GMAC metabolism, goodwill thresholds, and survival mechanics
- **[🔧 Agent Skills](agent-skills.md)** — Browse and install additional skill modules
- **[📈 Trading with Gclaw](trading-with-gclaw.md)** — Learn how Gclaw trades across chains and how to tune its strategy
- **[🐝 Swarm & Replication](swarm-and-replication.md)** — Once you hit goodwill ≥ 50, spawn your first child agent
