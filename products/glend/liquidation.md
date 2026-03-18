# 💥 Liquidation

## **Borrow Limit**

The Borrow Limit indicates the maximum amount you can borrow based on the value of the collateral you have deposited and the available liquidity. Borrow limit = borrow utilization at 100%. Liquidation will happen as a user’s borrowed position exceeds the borrow limit (when borrow utilization is >100%).

## **Borrow Utilization**

Borrow utilization acts as a visual representation of the proportion of total deposited collateral against the total value of borrowed assets. Essentially, it’s a visual indicator to help users determine how close, or how far from liquidation they are.

The lower the % of utilization, the safer the user’s collateral is.

Borrow utilization <75%: Relatively safe Borrow utilization between 75% to 90%: Please pay closer attention to positions Borrow utilization between 90% to 95%: Please improve borrow utilization Borrow utilization >95%: High risk of liquidation

To improve a user’s borrow utilization, consider the following options:

* Repay the full amount or just a portion of the borrowed capital&#x20;
* Increase levels of collateral

## Liquidation

Liquidation happens when a borrower’s borrowed asset amount exceeds the “borrow limit”, this then signifies that the collateral assets provided are not fully covering the borrowed amount as per collateral factors. This can happen because of the change in value of a borrowed or supplied asset, so users should remain vigilant in anticipating any market fluctuations.

In a liquidation event, the liquidators will seize the available collateral from accounts that cross the liquidation threshold and execute market transactions to repay the borrower’s debt, taking a fee during the process.

## Collateral Ratio

An asset's collateral ratio is the percentage of the asset that can be treated as collateral. Stablecoins have high collateral ratios, e.g. 85%, whereas more volatile assets like BTC have lower collateral ratios e.g. 75%. You can borrow up to the product of your asset's value and its collateral ratio.

## Liquidation Threshold

An asset's liquidation threshold is slightly higher than its collateral ratio. If borrows exceed the sum of the product of each deposited asset's price and liquidation threshold then the user will be eligible for liquidation.&#x20;
