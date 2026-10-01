---
description: GLend V2 smart contracts on Ethereum and Base, plus the legacy TenderFi contracts on Arbitrum that are not Gemach's
---

# 📜 Contract Addresses

Always check a contract address against this page and the block explorer before you approve or deposit anything.

## GLend V2 — Ethereum

<table><thead><tr><th width="446">Contract</th><th width="252">Address</th></tr></thead><tbody><tr><td>Comptroller (Unitroller)</td><td><pre><code>0x4a4c2A16b58bD63d37e999fDE50C2eBfE3182D58
</code></pre></td></tr><tr><td>Price Oracle</td><td><pre><code>0x4485f3e5a2fc2f693bdabd26d5fe81d4d4a06867
</code></pre></td></tr><tr><td>tUSDT</td><td><pre><code>0xfd7E506495fd921a17802Cf523279f01550BE8b6
</code></pre></td></tr><tr><td>tUSDC</td><td><pre><code>0x1C5215F2fb5417BdF9D93339b0caf20222f210f3
</code></pre></td></tr><tr><td>tETH</td><td><pre><code>0x6baeCC06B2faFD651B095ab3b7882AEe6EC4369D
</code></pre></td></tr><tr><td>tcbBTC</td><td><pre><code>0xF4faD7E54bF68344906C2b60fBCDB031cdeaDB52
</code></pre></td></tr><tr><td>tstETH</td><td><pre><code>0x6e9acC9D6ea3edE1acAE7Eeb2Be2dE1F8572Bc82
</code></pre></td></tr></tbody></table>

On Ethereum the Comptroller's `getAllMarkets()` also returns three older, unfunded markets that reuse the symbols tUSDT, tUSDC and tETH. Use only the addresses above.

## GLend V2 — Base

<table><thead><tr><th width="446">Contract</th><th width="252">Address</th></tr></thead><tbody><tr><td>Comptroller (Unitroller)</td><td><pre><code>0x4a4c2A16b58bD63d37e999fDE50C2eBfE3182D58
</code></pre></td></tr><tr><td>Price Oracle</td><td><pre><code>0x97f602E17ed4e765a6968f295Bdc3F6b4c1Ef93b
</code></pre></td></tr><tr><td>gUSDT</td><td><pre><code>0x6eAB9a4f7fDE8C1Ef0F62DA16549C80Bb7b7f853
</code></pre></td></tr><tr><td>gUSDC</td><td><pre><code>0xfd7E506495fd921a17802Cf523279f01550BE8b6
</code></pre></td></tr><tr><td>gETH</td><td><pre><code>0x1C5215F2fb5417BdF9D93339b0caf20222f210f3
</code></pre></td></tr><tr><td>gcbBTC</td><td><pre><code>0x920D3D27b17DCC16A7263d2ab176ac68C8385cd3
</code></pre></td></tr></tbody></table>

The Comptroller has the same address on Ethereum and Base. That is expected: they are separate deployments on separate networks.

## Not Gemach: legacy TenderFi contracts on Arbitrum

> ⚠️ **The Arbitrum lending markets are not GLend V2 and Gemach does not control them.**
> The Arbitrum money markets that earlier versions of these docs listed as GLend (Unitroller `0xeed247Ba513A8D6f78BE9318399f5eD1a4808F8e` and its `t` markets: tETH, tUSDC, tUSDT, tWBTC, tARB and the rest) are the legacy **TenderFi** protocol. Gemach never held the admin keys for those contracts and never took over their ownership. **Do not send funds to them.** The full list of legacy addresses is on the [Contract Addresses](contract-addresses.md) page so you can recognise them.

These are listed only so you can recognise them. **They are not GLend V2 contracts. Do not deposit into them.**

<table><thead><tr><th width="446">Contract</th><th width="252">Address</th></tr></thead><tbody><tr><td>Unitroller</td><td><pre><code>0xeed247Ba513A8D6f78BE9318399f5eD1a4808F8e
</code></pre></td></tr><tr><td>tETH</td><td><pre><code>0x0706905b2b21574DEFcF00B5fc48068995FCdCdf
</code></pre></td></tr><tr><td>tWETH</td><td><pre><code>0x242f91207184FCc220beA3c9E5f22b6d80F3faC5
</code></pre></td></tr><tr><td>tWBTC</td><td><pre><code>0x0A2f8B6223EB7DE26c810932CCA488A4936cF391
</code></pre></td></tr><tr><td>tUSDC</td><td><pre><code>0x068485a0f964B4c3D395059a19A05a8741c48B4E
</code></pre></td></tr><tr><td>tUSDT</td><td><pre><code>0x4A5806A3c4fBB32F027240F80B18b26E40BF7E31
</code></pre></td></tr><tr><td>tDAI</td><td><pre><code>0xB287180147EF1A97cbfb07e2F1788B75df2f6299
</code></pre></td></tr><tr><td>tFRAX</td><td><pre><code>0x27846A0f11EDC3D59EA227bAeBdFa1330a69B9ab
</code></pre></td></tr><tr><td>tARB</td><td><pre><code>0xC6121d58E01B3F5C88EB8a661770DB0046523539
</code></pre></td></tr><tr><td>tfsGLP</td><td><pre><code>0xFF2073D3810754D6da4783235c8647e11e43C943
</code></pre></td></tr><tr><td>tfsMLP</td><td><pre><code>0xd8d66f7b99caee7b764cb66cf9931cab49d4ab2c
</code></pre></td></tr><tr><td>tGMX</td><td><pre><code>0x20a6768F6AABF66B787985EC6CE0EBEa6D7Ad497
</code></pre></td></tr><tr><td>tMAGIC</td><td><pre><code>0x4180f39294c94F046362c2DBC89f2DF7786842c3
</code></pre></td></tr><tr><td>tUNI</td><td><pre><code>0x8b44D3D286C64C8aAA5d445cFAbF7a6F4e2B3A71
</code></pre></td></tr><tr><td>tLINK</td><td><pre><code>0x87D06b55e122a0d0217d9a4f85E983AC3d7a1C35
</code></pre></td></tr><tr><td>tgmdBTC</td><td><pre><code>0xB60EF53BA18Bd85Ab642c2F78dF13e7aBCCdCb9c
</code></pre></td></tr><tr><td>tgmdETH</td><td><pre><code>0xB5dBDb01B08bff12E822EB28259ECCEb6cC91529
</code></pre></td></tr><tr><td>tgmdUSDT</td><td><pre><code>0x80aEFB7dAde25542cc2f558Ee605aC2FC974Ceb9
</code></pre></td></tr><tr><td>tgmdUSDC</td><td><pre><code>0xe4843e44342617024F6b9d615dFfBe8858F8Ea16
</code></pre></td></tr></tbody></table>
