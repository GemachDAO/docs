---
description: Security best practices for Gemach users — self-custody principles, wallet security, phishing protection, and how Gemach handles your keys.
---

# 🔐 General Security Notes

Security in DeFi starts with understanding what you control and what the protocol controls. This guide covers both — plus practical steps to protect yourself from the most common threats.

---

## The Self-Custody Guarantee

Every Gemach product is self-custodial. This means:

- **Your private keys never leave your device** — Gemach servers do not store, transmit, or have access to your private keys or seed phrases
- **You sign every transaction** — No Gemach product can move your funds without an explicit transaction approval from your wallet
- **Web3Auth session keys** — When you use social login (Google, Apple) on GDex Pro, Web3Auth generates a session key encrypted with AES-256. Gemach receives only a signed proof of authentication, not the key itself. You can export your key material from Web3Auth at any time.

> **Note:** "Self-custodial" means you bear full responsibility for your wallet security. Gemach cannot recover lost seed phrases, reverse transactions, or freeze stolen funds on your behalf.

---

## Wallet Security Fundamentals

### Seed Phrase / Private Key
- Write your seed phrase on paper and store it offline — never in a text file, email, password manager, or cloud storage
- Never share your seed phrase or private key with anyone, for any reason — no Gemach support agent will ever ask for it
- Use a hardware wallet (Ledger, Trezor) for significant holdings

### Two-Wallet Strategy
Maintain two separate wallets:
- **Cold wallet** — stores the majority of your assets; rarely connected to dApps; ideally a hardware wallet
- **Hot wallet** — used for active trading and DeFi interactions; funded only with what you need for current activity

### Wallet Software
- Download wallet software only from official sources (MetaMask from metamask.io, Phantom from phantom.app, etc.)
- Keep wallet extensions and firmware up to date
- Audit installed browser extensions — malicious extensions can read wallet data

---

## Approved Gemach Domains

Only interact with Gemach products through these official domains. Bookmark them and verify the URL before connecting your wallet:

| Product | Official Domain |
|---|---|
| GDex Pro | `gdex.pro` |
| Gemach (main site) | `gemach.io` |
| Gclaw | `gclaw.gemach.io` |
| GLend | `glend.gemach.io` |
| GLoans | `stake.gemach.io` |
| GBot | Telegram only: `@Gemach_Bot` |
| GScanner | Telegram only: `@gbotscanner` |
| Gemach Support | Telegram only: `@GBotSupportBot` |

> ⚠️ **Warning:** Phishing sites impersonate legitimate dApps with near-identical domain names (e.g., `gdex-pro.com`, `gemach-io.com`). Always verify you are on the exact official domain before connecting your wallet or approving transactions.

---

## Contract Address Verification

Before interacting with any smart contract, verify the address matches Gemach's published addresses:

- GLend contract addresses: [products/glend/contract-addresses.md](../products/glend/contract-addresses.md)
- GMAC token: `0xd96e84ddbc7cbe1d73c55b6fe8c64f3a6550deea` (Ethereum)
- Always cross-reference contract addresses on [Etherscan](https://etherscan.io), [Arbiscan](https://arbiscan.io), or the relevant block explorer before approving interactions

---

## Transaction Safety

### Before Signing Any Transaction

1. **Check the domain** in your browser bar — confirm it matches the official Gemach domain
2. **Read the transaction details** — your wallet will show the contract address, function name, and parameters before you sign
3. **Verify the contract** — compare the contract address shown in your wallet with Gemach's published addresses
4. **Check the operation** — common safe operations include `swap`, `deposit`, `approve`, `borrow`. Be cautious of `setApprovalForAll` or unfamiliar function names
5. **Revoke unused approvals** — use [revoke.cash](https://revoke.cash) to audit and revoke old token approvals you no longer need

### Token Approvals

When you approve a token for spending (required before most DEX trades), you are granting a contract permission to spend up to the approved amount. Best practices:

- Approve only the amount needed for the current transaction (exact approval)
- Revoke approvals for contracts you no longer use
- Never approve unlimited amounts to unfamiliar contracts

---

## Phishing and Social Engineering

Gemach support staff will **never**:
- Ask for your seed phrase or private key
- Ask you to approve a transaction "to verify your wallet"
- DM you first to offer help — all official support goes through [@GBotSupportBot](https://t.me/GBotSupportBot)
- Send you an unsolicited link and ask you to connect your wallet

Common scam patterns to watch for:
- **Fake support accounts** on Telegram/Discord with usernames similar to official accounts
- **Airdrop scams** — "connect your wallet to claim your GMAC" on unofficial domains
- **Urgent action requests** — "your funds are at risk, act now" messages designed to create panic
- **Fake token contracts** — tokens named GMAC or GLEND on incorrect contract addresses

---

## Smart Contract Risk

All DeFi protocols carry inherent smart contract risk. Gemach mitigates this by:

- Using audited third-party infrastructure wherever possible: Enzyme (GVault), Liquity (GLoans), TokenSets (GFund), HyperLiquid (GDex Pro perps)
- Publishing audit reports for GLend contracts
- Maintaining bug bounty programs for critical infrastructure

No audit eliminates all risk. Do not invest more than you can afford to lose in any DeFi protocol.

---

## Reporting Security Issues

If you discover a security vulnerability in any Gemach product:

1. **Do not post it publicly** — this could be exploited before a fix is deployed
2. **Contact the team directly** via the official Telegram support bot: [@GBotSupportBot](https://t.me/GBotSupportBot) or through the Gemach Discord: [discord.com/invite/QDHsGPQdx3](https://discord.com/invite/QDHsGPQdx3)
3. Responsible disclosures are recognized and rewarded by the Gemach DAO
