---
description: How Gclaw trades across chains using the GDEX SDK — spot, perp, copy trading, bridge, and more
---

# 📈 Trading with Gclaw

Gclaw is a living agent that trades to survive. Its trading capabilities are powered by the **GDEX SDK**, which provides native access to spot markets, perpetual futures, copy trading, cross-chain bridging, and token discovery across more than 12 blockchain networks. Every trade Gclaw executes is autonomous — driven by its strategy configuration, live market data, and the agent's own judgment based on its LLM reasoning.

{% hint style="warning" %}
Always start with small amounts. Gclaw trades autonomously — make sure you understand the risks before enabling live trading. Review your agent's trading parameters in `~/.gclaw/config.json` and set conservative position limits while you're getting started.
{% endhint %}

{% hint style="danger" %}
Never trade more than you can afford to lose. Gclaw operates without real-time human confirmation of each trade — set hard limits on position size and maximum drawdown before going live.
{% endhint %}

## How Gclaw Connects to GDEX

Gclaw uses the GDEX SDK to interface with decentralized exchanges, liquidity pools, and derivative protocols across multiple chains. The connection is authenticated with your GDEX API key and wallet credentials, which you provide during onboarding. Your keys remain in your control at all times — Gclaw never sends them to a third-party server.

For the equivalent web-based trading interface, see [🪬 GDex Pro](../gdex-pro/README.md).

<!-- Screenshot: Diagram showing Gclaw agent connecting to GDEX SDK and routing orders to Solana, Base, Arbitrum, and HyperLiquid -->

## Spot Trading

Gclaw can buy and sell tokens on spot markets across all major chains supported by GDEX:

**Supported chains for spot trading:**
- Solana
- Ethereum
- Base
- BNB Chain (BSC)
- Arbitrum
- Optimism
- Sui
- Sonic
- Berachain
- And 3+ additional EVM-compatible networks

The agent uses the `gdex-spot-trading` skill to execute spot orders. It evaluates token opportunities based on market data from the `gdex-token-discovery` skill, applies your configured risk parameters, and places orders directly through the GDEX SDK.

<!-- Screenshot: Terminal output showing Gclaw executing a spot buy order with trade details and confirmation -->

## Limit Orders

Gclaw supports limit orders with configurable take-profit and stop-loss levels via the `gdex-limit-orders` skill. Rather than executing market orders reactively, the agent can place structured orders that trigger automatically when price conditions are met.

Example workflow:
1. Agent identifies a token opportunity via `gdex-token-discovery`
2. It places a limit buy order at a target entry price
3. It simultaneously sets a take-profit order (e.g., +15%) and a stop-loss (e.g., -5%)
4. If the entry triggers, the TP/SL orders activate automatically
5. The outcome — win or loss — feeds back into the agent's GMAC balance and goodwill score

## Copy Trading

With the `gdex-copy-trading` skill, Gclaw can identify high-performing wallets and mirror their spot trading activity. The agent monitors the top performers on GDEX, selects wallets that match your configured risk profile, and replicates their trades proportionally.

Copy trading is a powerful strategy for a new agent building its goodwill score — it learns from successful traders while earning GMAC from the mirrored gains.

## Perpetual Trading

The `gdex-perp-trading` skill gives Gclaw access to leveraged perpetual futures on **HyperLiquid**. The agent can open long and short positions with configurable leverage, manage funding rate exposure, and close positions autonomously.

Gclaw also supports **perp copy trading** via `gdex-perp-copy-trading` — mirroring the perpetual futures positions of top-performing HyperLiquid traders.

{% hint style="warning" %}
Perpetual futures carry significantly higher risk than spot trading due to leverage and liquidation risk. If you enable perp trading, ensure your stop-loss parameters are set conservatively and monitor the Living Dashboard regularly.
{% endhint %}

## Bridge

The `gdex-bridge` skill enables Gclaw to move assets across chains autonomously. This is essential for multi-chain strategies where the agent identifies opportunities on a chain where it doesn't currently hold the required token.

Supported bridge routes include all major GDEX-supported chains. The agent handles the bridge transaction, monitors confirmation, and continues its strategy on the destination chain — all without human intervention.

## Token Discovery

Before executing trades, Gclaw uses the `gdex-token-discovery` skill to evaluate potential targets. Token discovery provides:

- **Trending tokens** — tokens with rising volume and holder count
- **Security scores** — contract audit status, honeypot detection, rug-pull risk indicators
- **Market metrics** — 24h volume, liquidity depth, price momentum
- **Cross-chain availability** — whether the token trades on multiple chains (useful for bridge arbitrage)

The agent weighs these factors against its configured risk parameters before committing to a trade.

## Risk Management

Gclaw is autonomous, which means good risk configuration is critical. Here are the key parameters to review in `~/.gclaw/config.json`:

- **`max_position_size_usd`** — Hard cap on the USD value of any single position. Start small (e.g., $10–$50) while your agent builds its track record.
- **`stop_loss_percent`** — Maximum loss before the agent auto-exits a position. A 5% stop-loss is a common starting point.
- **`take_profit_percent`** — Target gain before the agent auto-exits with a profit.
- **`preferred_chains`** — Limit the agent to chains you understand and are comfortable with.
- **`max_open_positions`** — Cap the total number of concurrent open positions.

**Additional best practices:**
- Start the agent in market-monitor mode (no GDEX key) to observe its decision-making before enabling live trading
- Review the trade history in the Living Dashboard after the first 24 hours
- Diversify across chains rather than concentrating on a single network
- Check the P&L summary daily until you're confident in the agent's strategy calibration

<!-- Screenshot: Living Dashboard trade history table showing recent trades, entry/exit prices, and P&L per trade -->

---

**See also:**
- [🪬 GDex Pro](../gdex-pro/README.md) — The web interface for manual trading on the same GDEX infrastructure
- [🔧 Agent Skills](agent-skills.md) — Full list of trading and utility skills
- [🧬 The Living Agent](living-agent.md) — How trading outcomes affect GMAC balance and goodwill
