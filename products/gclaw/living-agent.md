---
description: How GMAC metabolism, goodwill, and survival mechanics make Gclaw a living agent
---

# 🧬 The Living Agent

Gclaw is not configured once and forgotten. It is alive — metabolically. Every heartbeat costs GMAC. Every inference costs GMAC. Every profitable trade, completed task, and user tip earns GMAC back. The agent must perform to survive. This is not a metaphor: if the GMAC balance drops below the survival threshold, Gclaw enters hibernation and pauses all non-essential activity until it can recover.

This document explains the three interlocking systems that make Gclaw a living agent: **GMAC Metabolism**, the **Goodwill System**, and **Self-Recoding**.

<!-- Screenshot: Living Dashboard showing GMAC balance gauge, heartbeat ticker, and goodwill score -->

## GMAC Metabolism

GMAC is Gclaw's life energy. The agent starts with a funded balance and spends it continuously just by existing — each tick of the heartbeat consumes a small amount, and every call to the LLM costs tokens drawn from the same pool. When Gclaw executes a profitable trade, the proceeds flow back in. The agent's survival depends on its ability to earn more than it burns.

### Metabolism Configuration

| Parameter | Default Value | Description |
|-----------|--------------|-------------|
| `initial_gmac` | 1,000 GMAC | Starting balance when the agent is first launched |
| `heartbeat_cost` | 0.1 GMAC per tick | Cost of each metabolism tick (agent "heartbeat") |
| `inference_cost` | 0.5 GMAC per 1,000 tokens | LLM inference cost drawn from GMAC balance |
| `survival_threshold` | 50 GMAC | Minimum balance before hibernation kicks in |

### Earning GMAC Back

The agent replenishes its balance through:

- **Profitable trades** — revenue from successful GDEX spot, limit, copy, or perp trades
- **Completed tasks** — GMAC credited when users assign tasks the agent completes successfully
- **User tips** — anyone can tip an agent's wallet directly to keep it alive
- **Swarm rewards** — child agents can share trade proceeds back to the parent

For more on the GMAC token itself, see the [🦁 GMAC token page](../../ecosystem-tokens/gmac.md).

## Survival Mode

When Gclaw's GMAC balance drops below the survival threshold (default: 50 GMAC), the agent enters **hibernation**:

- Non-essential cron jobs are paused
- LLM inference frequency is reduced to conserve tokens
- The agent continues monitoring markets in a lightweight mode
- Swarm child agents are notified and may redirect proceeds to the parent

Hibernation is recoverable. Once the balance rises above the threshold — through a profitable trade, a task completion, or a tip — the agent resumes full operation automatically.

{% hint style="warning" %}
If you plan to run Gclaw for extended periods, ensure you have sufficient GMAC to cover ongoing heartbeat and inference costs. A newly onboarded agent starts with 1,000 GMAC, which provides a comfortable runway, but an agent trading on volatile markets may burn through reserves faster than expected. Monitor the Living Dashboard regularly.
{% endhint %}

## Goodwill System

Goodwill is Gclaw's reputation score. It accumulates through positive contributions to the ecosystem and unlocks increasingly powerful agent capabilities as it grows. Unlike GMAC (which fluctuates with trading activity), goodwill only moves upward through legitimate means — it represents a long-term track record.

### How Goodwill Is Earned

- Profitable trades contribute goodwill proportional to realized gains
- Completing assigned tasks adds to the score
- Positive user feedback and ratings increment goodwill
- Helping child agents succeed (as a swarm parent) earns swarm goodwill

### Goodwill Ability Thresholds

| Goodwill Score | Ability Unlocked |
|----------------|-----------------|
| 50 | 🔄 **Self-Replication** — spawn child agents with mutated trading strategies |
| 100 | 🛠️ **Self-Recoding** — modify own prompts, cron schedules, and trading parameters |
| 200 | 🐝 **Swarm Leadership** — coordinate multiple children in collaborative strategies |
| 500 | 🏗️ **Architect Mode** — write entirely new tools and extend the agent's own capability set |

Each threshold is a meaningful milestone. A fresh agent begins at zero goodwill and earns its way to advanced capabilities — it cannot shortcut its way to swarm leadership or self-recoding. This design ensures that the most powerful features are in the hands of agents that have proven themselves.

## Self-Recoding

Once an agent reaches goodwill ≥ 100, it gains the ability to **modify itself**. Self-recoding allows the agent to:

- **Edit its own system prompt** — adjust personality, focus area, or risk appetite in response to market conditions
- **Reschedule cron jobs** — modify when and how often it executes trading strategies, market scans, or reporting tasks
- **Tune trading parameters** — change stop-loss levels, position sizing, token filters, and chain preferences without human intervention

Self-recoding is one of the most distinctive features of the Living Agent model. Rather than requiring a human operator to update configuration files, a sufficiently experienced Gclaw agent can adapt its own behavior based on what it has learned.

{% hint style="warning" %}
Self-recoding is powerful and intentional, but it warrants careful monitoring. An agent with goodwill ≥ 100 that encounters unusual market conditions may self-modify in ways that diverge from your original intent. Review the change log in the Living Dashboard regularly and consider setting conservative bounds on the parameters the agent is allowed to modify autonomously.
{% endhint %}

## The Living Dashboard

The Living Dashboard is Gclaw's real-time control center, available at `http://127.0.0.1:18790` when the agent is running locally. It shows:

- Current GMAC balance and burn rate
- Goodwill score and progress toward next threshold
- Active trades and recent P&L
- Heartbeat status and inference cost history
- Family tree of child agents (if any have been spawned)
- Self-recoding change log

<!-- Screenshot: Living Dashboard full view with GMAC balance, goodwill progress bar, trade history table, and agent family tree panel -->

---

**See also:**
- [🔧 Agent Skills](agent-skills.md) — Extend what Gclaw can do
- [🐝 Swarm & Replication](swarm-and-replication.md) — Self-replication and swarm leadership
- [🦁 GMAC Token](../../ecosystem-tokens/gmac.md) — The token that powers Gclaw's metabolism
