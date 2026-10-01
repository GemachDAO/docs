# ◀️ Borrow

> ⚠️ **The Arbitrum lending markets are not GLend V2 and Gemach does not control them.**
> The Arbitrum money markets that earlier versions of these docs listed as GLend (Unitroller `0xeed247Ba513A8D6f78BE9318399f5eD1a4808F8e` and its `t` markets: tETH, tUSDC, tUSDT, tWBTC, tARB and the rest) are the legacy **TenderFi** protocol. Gemach never held the admin keys for those contracts and never took over their ownership. **Do not send funds to them.** The full list of legacy addresses is on the [Contract Addresses](contract-addresses.md) page so you can recognise them.

You can use deposited assets as collateral to borrow against. The amount you may borrow in relation to your collateral depends on the type of asset being used as collateral. You can find your borrow limit in your dashboard.

Assets and volume available to borrow depend on the liquidity of the supplied asset. If there is no liquidity, you would need to wait for a new user or the platform to supply more, or for a loan to be repaid.

GLend V2 borrowing markets are USDT, USDC, ETH, cbBTC and stETH on Ethereum, and USDT, USDC, ETH and cbBTC on Base. The availability of each asset and its borrow rate depend on liquidity and demand in that market.

### Step 1

Click the asset you wish to borrow. There must be sufficient liquidity for the asset and you must not have exceeded your borrow limit for you to be able to borrow an asset.

### Step 2

Enter the amount you wish to borrow and then click the “Borrow” button. You will then need to sign a transaction and pay gas fees in your wallet to complete the borrow transaction.

You may also need to enable the asset in your wallet and pay a gas fee before you can complete the transaction.

You can find the amount of your outstanding borrowed assets in your dashboard, along with any remaining borrow limit.

