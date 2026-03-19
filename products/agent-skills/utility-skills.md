---
description: Complete reference for all 8 utility skills — extend your agent beyond trading into payments, GitHub, hardware, and more
---

# 🛠️ Utility Skills

The 8 utility skills extend Gemach agents beyond DeFi trading into a broader range of autonomous capabilities. They are designed to complement GDEX trading skills in compound workflows — for example, summarizing a trade report and pushing it to a GitHub repository, or monitoring hardware health and suspending the agent (entering hibernation mode) if the system overheats.

## Skill Catalog

### `skill-creator`

**Purpose:** A meta-skill that helps agents and users design, scaffold, and write new `SKILL.md` files.

**Key capabilities:**
- Accept a natural-language description of a desired capability
- Generate a complete, correctly structured `SKILL.md` scaffold
- Suggest appropriate tools, parameters, and error conditions
- Create matching helper script stubs (Node.js, Python, or shell)
- Validate the output against the SKILL.md format specification

**Why it matters:** This skill is the entry point for growing the skills ecosystem. Instead of writing a new skill from scratch, you describe what you want and the agent drafts the structure for you. It bootstraps the community contribution loop.

**Example usage:**

```
You: Create a skill that monitors a Telegram bot for incoming messages
     and forwards them to a webhook URL.

Agent: I've scaffolded workspace/skills/telegram-webhook/SKILL.md with
       a listen_for_messages tool and helpers/poll-telegram.js. Review
       the file and let me know what to adjust.
```

**Example prompts:**
```
"Create a new skill for fetching DEX prices from Birdeye."
"Scaffold a skill that lets me interact with the Airtable API."
"Help me write a skill for monitoring my Solana wallet for incoming transfers."
```

---

### `github`

**Purpose:** Interact with GitHub repositories as part of agent-automated workflows.

**Key capabilities:**
- Create, read, update, and close issues
- Post comments on issues and pull requests
- Push file changes to a repository branch
- Create and merge pull requests
- Read file contents from any accessible repository
- List repository branches, commits, and open PRs

**Common use cases:**

| Workflow | Description |
|----------|-------------|
| Trade log publishing | Auto-commit daily trade reports to a private repo |
| Bug reporting | Open GitHub issues when SDK errors are detected |
| Strategy versioning | Commit updated strategy parameters to version control |
| Docs updating | Push skill documentation updates automatically |

**Example prompts:**
```
"Open a GitHub issue in my-org/my-repo with the title 'SDK rate limit exceeded'."
"Commit today's trade log to the reports/ directory in my trading-logs repo."
"Show me all open PRs in GemachDAO/Gclaw."
"Post a comment on PR #42 in my-repo saying the tests passed."
```

**Authentication:** Requires a GitHub personal access token (PAT) or GitHub App credentials configured in the agent's environment.

---

### `summarize`

**Purpose:** Condense any text — documents, trade logs, chat histories, market reports — into structured, actionable summaries.

**Key capabilities:**
- Summarize long documents to a configurable length (brief, detailed, or bullet points)
- Extract key insights, decisions, and action items from conversation logs
- Generate trade recap reports from raw transaction histories
- Condense market analysis reports into executive summaries
- Multi-document summarization with cross-source synthesis

**Output formats:**

| Format | Use Case |
|--------|----------|
| `brief` | 2–3 sentence summary for quick status checks |
| `bullets` | Bulleted list of key points — best for action items |
| `detailed` | Structured multi-paragraph summary for full context |
| `report` | Formatted report with sections and headings |

**Example prompts:**
```
"Summarize my last 7 days of trade activity into a weekly report."
"Give me a 3-bullet summary of this market analysis."
"Condense this 5000-word whitepaper into the key points."
"Create a summary of today's agent activity log."
```

---

### `hardware`

**Purpose:** Monitor and respond to hardware health metrics on the host system — especially useful for agents running on low-resource boards.

**Key capabilities:**
- Monitor CPU usage, memory usage, disk space, and temperature
- Set alert thresholds that trigger agent actions (slow down inference, hibernate, send notification)
- Query current system metrics on demand
- Log hardware health over time for trend analysis
- Trigger graceful hibernation when resources are critically low

**Threshold actions:**

| Condition | Default Threshold | Default Action |
|-----------|------------------|----------------|
| CPU > 90% for 60s | 90% | Pause non-critical tasks |
| Memory > 85% | 85% | Free caches, reduce context window |
| Temperature > 80°C | 80°C | Throttle inference; alert |
| Disk < 5% free | 5% | Alert; do not write new files |

**Example prompts:**
```
"What's the current CPU and memory usage?"
"Alert me if the temperature on my Raspberry Pi exceeds 75°C."
"Check if I have enough free disk space to store today's trade logs."
"Put the agent in low-resource mode — I'm running other workloads."
```

**Typical deployment:** Hardware monitoring is most valuable on agents running on single-board computers (Raspberry Pi, Orange Pi, Radxa) where thermal and memory limits are real constraints.

---

### `weather`

**Purpose:** Fetch current weather conditions and forecasts for use in multi-agent workflows and context-aware decision-making.

**Key capabilities:**
- Current conditions: temperature, humidity, wind, UV index
- Hourly and 7-day forecasts
- Weather alerts and severe conditions
- Location lookup by city name, coordinates, or IP geolocation

**Use in compound workflows:**

Weather data becomes useful when combined with other skills. For example, an agent managing agricultural equipment might check weather before scheduling irrigation; or a DeFi event agent might note weather conditions when logging real-world context for a trade journal.

**Example prompts:**
```
"What's the weather in San Francisco right now?"
"Will it rain in London tomorrow afternoon?"
"Get the 7-day forecast for 37.7749,-122.4194."
"Are there any weather alerts for New York City?"
```

---

### `tmux`

**Purpose:** Create, attach to, and manage tmux sessions — enabling agents to run multiple parallel processes in a structured terminal environment.

**Key capabilities:**
- Create new named tmux sessions and windows
- Split panes horizontally and vertically
- Send commands to specific panes without attaching
- Capture pane output for reading by the agent
- List all active sessions and their status
- Kill specific sessions or windows

**Typical use cases:**

| Use Case | Description |
|----------|-------------|
| Multi-agent orchestration | Run multiple Gclaw agents side by side in named sessions |
| Log monitoring | Keep a log-tail window alongside the main agent |
| Background processes | Keep helper processes alive across agent restarts |
| Development workflows | Run test suite, watcher, and REPL in parallel panes |

**Example prompts:**
```
"Start a new tmux session called 'trading-agent'."
"Open a second window in my current tmux session for monitoring logs."
"Send 'gclaw monitor' to the second pane."
"Show me the output from the log-monitor pane."
"Kill the old development session."
```

---

### `tempo-payment`

**Purpose:** Handle [Tempo](https://www.tempo.finance/) payment protocol flows — enabling agents to send, receive, and verify Tempo-based payments as part of automated transaction workflows. Tempo is a programmable payment protocol built for autonomous agents, allowing machines to transact with each other using on-chain payment channels.

**Key capabilities:**
- Initiate Tempo payment requests with configurable amounts and recipients
- Monitor payment status and confirm receipt
- Integrate Tempo payments into multi-step agent workflows
- Handle payment callbacks and webhooks
- Record payment history and receipts

**Example prompts:**
```
"Send a Tempo payment of 10 USDC to wallet 0xRecipient..."
"Check if payment ID tempo_xyz123 has been confirmed."
"Set up a recurring Tempo payment of 5 USDC per day to my service wallet."
```

---

### `x402`

**Purpose:** Process x402 micropayment protocol flows — letting agents autonomously pay for external APIs and services without human intervention.

**Key capabilities:**
- Detect x402 payment challenges from HTTP 402 responses
- Automatically construct and sign payment proofs
- Pay for API access with configurable per-request budget limits
- Track cumulative spend across all x402-protected endpoints
- Reject payment requests that exceed configured spend limits

**What x402 enables:** The x402 protocol extends HTTP with native micropayment support. When an agent requests a resource and receives a `402 Payment Required` response, the x402 skill handles the payment challenge automatically — the agent pays and retries without human involvement. This enables truly autonomous access to paid data feeds, AI inference endpoints, and other metered services.

**Example prompts:**
```
"Fetch the premium market data report — handle the payment automatically."
"What's my total x402 spend this session?"
"Set a budget limit of 1 USDC per hour for all x402 payments."
"Use x402 to access the Chainlink premium data feed."
```

---

## Combining Utility and Trading Skills

Utility skills are designed to compose naturally with GDEX trading skills. Some powerful combinations:

| Combination | Workflow |
|-------------|----------|
| `gdex-token-discovery` + `summarize` | Discover trending tokens, summarize findings into a briefing |
| `gdex-portfolio` + `github` | Track portfolio daily and commit reports to a private repo |
| `gdex-spot-trading` + `x402` | Buy tokens using paid data feeds for signal generation |
| `hardware` + `gdex-trading` | Monitor hardware health and pause trading if system overheats |
| `skill-creator` + any skill | Use the agent to extend itself with new capabilities |
| `tmux` + any skill | Run multiple trading strategies in parallel sessions |

---

**See also:**
- [📈 GDEX Trading Skills](gdex-trading-skills.md) — the 14 trading skills reference
- [✏️ Creating Custom Skills](creating-custom-skills.md) — add your own utility skills
- [🧩 Agent Skills Overview](README.md) — full catalog and loading priority
