---
description: Gemach's lending and borrowing protocol on Ethereum and Base
---

# 🧮 GLend

> ⚠️ **The Arbitrum lending markets are not GLend V2 and Gemach does not control them.**
> The Arbitrum money markets that earlier versions of these docs listed as GLend (Unitroller `0xeed247Ba513A8D6f78BE9318399f5eD1a4808F8e` and its `t` markets: tETH, tUSDC, tUSDT, tWBTC, tARB and the rest) are the legacy **TenderFi** protocol. Gemach never held the admin keys for those contracts and never took over their ownership. **Do not send funds to them.** The full list of legacy addresses is on the [Contract Addresses](contract-addresses.md) page so you can recognise them.

GLend V2 is Gemach's lending and borrowing protocol. It runs on **Ethereum** and **Base** and uses the Compound V2 money-market model: you supply an asset to earn variable interest, and you can use what you supplied as collateral to borrow other assets. Positions stay in your own wallet's control and are managed entirely on-chain.

App: [glendv2.gemach.io](https://glendv2.gemach.io/)

## Markets

| Network | Markets |
|---|---|
| Ethereum | USDT, USDC, ETH, cbBTC, stETH |
| Base | USDT, USDC, ETH, cbBTC |

Interest rates adjust algorithmically with supply and demand in each market. Contract addresses for every market are on the [Contract Addresses](contract-addresses.md) page.
