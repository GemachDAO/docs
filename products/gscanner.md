---
description: GScanner is a real-time token deployment scanner that alerts traders the moment new tokens launch and trading opens — available on Telegram.
---

# 📟 GScanner

GScanner is Gemach's real-time token monitoring and alert service, accessible directly through Telegram. It watches every new token deployment and liquidity pair creation on supported chains, then pushes instant notifications so traders can act before the crowd.

---

## How It Works

GScanner monitors on-chain events continuously:

1. **New token deployed** — a notification fires the moment a new token contract is deployed, before trading even opens
2. **Trading enabled** — a second alert fires when the token's liquidity pair is created and trading becomes possible — your signal to snipe with GBot or GDex Pro
3. **Instant delivery** — alerts arrive in Telegram within seconds of the on-chain event

This two-stage alert system gives you a head start: you see the token before it is tradeable, then get the entry signal the moment the pair opens.

---

## Accessing GScanner

GScanner is free to subscribe to on Telegram:

**[Subscribe to @gbotscanner](https://t.me/gbotscanner)**

---

## Signal Types

| Alert | Trigger | What to Do |
|---|---|---|
| 🆕 New Token Deployed | Contract creation detected on-chain | Research the token — check deployer wallet, initial supply |
| 🟢 Trading Enabled | Liquidity pair created | Execute entry via GBot or GDex Pro if the token passes your criteria |

---

## Using GScanner with GBot and GDex Pro

GScanner is most powerful when used alongside Gemach's trading tools:

- **GScanner + GBot** — Configure [GBot](gbot/README.md) to auto-snipe tokens flagged by GScanner based on your filters (min liquidity, holder count, contract verification status)
- **GScanner + GDex Pro** — When a GScanner alert fires, open [GDex Pro](gdex-pro/README.md) and use the Discovery feature or Memescope to analyze the token before committing capital

> ⚠️ **Warning:** New token launches carry significant risk. Many are honeypots, rug pulls, or low-quality projects. Always verify contract addresses, check deployer history, and never invest more than you can afford to lose in new token launches.

---

## Resources

| Link | Description |
|---|---|
| [@gbotscanner](https://t.me/gbotscanner) | Subscribe to GScanner alerts on Telegram |
| [GBot](gbot/README.md) | Automated Telegram trading bot — pairs perfectly with GScanner |
| [GDex Pro Discovery](gdex-pro/features/discovery-feature.md) | Token discovery and research tool in GDex Pro |
