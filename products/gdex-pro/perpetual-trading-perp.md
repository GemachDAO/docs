# Perpetual Trading (Perp)

### Overview

The **Perpetual Trading (Perp)** feature on GDex Pro lets you copy professional perpetual (perp) traders from Hyperliquid or open your own leveraged long and short positions. Perp copy trading automates your trades based on another wallet’s actions, while the manual trade interface offers advanced tools such as limit orders, cross/isolated margin and leverage.

### Accessing Perp Trading

1. **Navigate to the Perp Page:** From the GDex dashboard, open the _Perp_ link in the top navigation.
2. **Connect Your Wallet:** Ensure your wallet is connected. Perp copy trading uses USDC on the **Arbitrum** network. If you’re connected to another network, you’ll see a notice asking you to switch to Arbitrum.
3. **Deposit USDC:** In the _Hyperliquid Copytrade → Deposit_ tab, click **Deposit USDC** and follow the prompt to add funds. Deposits and withdrawals are only available on Arbitrum, so you must switch networks before depositing or withdrawing funds.

> **Note:** Without a deposit, you can still browse top traders, but you cannot execute copy trades or open positions.

### Creating a Perp Copy Trade

1. Go to **Hyperliquid Copytrade → Create Copy Trade**.
2. **Inverse Copy Trade:** Choose _Yes_ to invert the trader’s actions (their buys become your sells) or _No_ to mirror exactly.
3. **Targeted Address:** Enter the wallet address you want to copy.
4. **Label:** Give your strategy a name to identify it in your dashboard.
5. **Position Type:** Choose **Fixed amount** to define a USDC amount per order, or **Fixed Ratio** to allocate a percentage of your margin to each trade.
6. **Margin per Order (USDC):** Enter how much USDC you’re willing to allocate per order.
7. **Risk Management:** Optional fields let you set a **Take profit** percentage and **Stop loss** percentage and specify whether to automatically close the position when the copied trader closes theirs.
8. Click **Create** to start copying that trader. Your strategy will now appear in the **Copy Trade List** tab.

### Managing Your Copy Trades

* **Copy Trade List:** Shows all active copy‑trade bots you’ve created. Each entry includes the target address, your margin settings and current PnL. You can stop or edit a copy trade here.
* **Active Positions:** Displays open positions across all copied traders, with columns for coin, size, position value, entry price, mark price, PNL (ROE %), liquidation price and margin. Use **Close All** to exit all positions.
* **Trading Activity:** Provides a chronological log of every trade executed by your copy‑trade bots, including time, coin, direction (open/close long/short), price, your size, the copied trader and realised PnL.
* **Withdraw:** Lets you withdraw USDC back to your wallet. Withdrawals are only available on the Arbitrum network.

### Manual Perp Trading (Hyperliquid Trade)

For experienced traders, the **Hyperliquid Trade** tab provides a full-featured perpetual trading terminal. Key elements include:

* **TradingView chart:** A real‑time chart with standard drawing tools.
* **Market vs Limit orders:** You can choose between market and limit orders and toggle between **Cross** (shared margin across positions) and **40×** isolated margin with adjustable leverage.
* **Long vs Short:** Buttons to open buy/long or sell/short positions.
* **Size:** Enter your position size in USD or coin units. Optional toggles let you set _Reduce Only_ orders or add **Take Profit/Stop Loss** targets.
* **Deposit/Withdraw:** Buttons to move USDC in and out of your trading account. Remember that deposits and withdrawals must be done on the Arbitrum network.
* **Positions & History:** Tabs below the chart display open positions, open orders, trade history, funding history and order history.

### Risk Considerations

Perpetual futures are leveraged instruments and can amplify both gains and losses. Before copying a trader or opening a position:

* Only deposit funds you can afford to lose.
* Review the target wallet’s historical performance and adjust your margin per order accordingly.
* Use take‑profit and stop‑loss settings to manage risk.
* Remember that network fees and funding rates can affect your net PnL.

This Perp functionality adds powerful leverage trading to GDex Pro. Make sure to understand the mechanics and risks before participating.
