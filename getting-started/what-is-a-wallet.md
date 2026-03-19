# 👛 What is a Crypto Wallet?

A crypto wallet is one of the most important concepts to understand before using any DeFi platform. Here's the short version: **a wallet is like a bank account that only you control.** No bank can freeze it, no company can close it, and nobody else can access it — as long as you keep your private key safe.

## The mailbox analogy

The easiest way to understand a crypto wallet is to think of a mailbox:

- Your **public address** is like your mailbox address — anyone can see it and send tokens to it. It's safe to share.
- Your **private key** (or seed phrase) is like the key to open the mailbox — only you should ever have it. If someone else gets it, they can take everything inside.

Your wallet address looks something like this:
`0x742d35Cc6634C0532925a3b8D4C9C2e6e4b6e42F` (Ethereum) or
`5Cz8...k7Wm` (Solana)

These are perfectly safe to share when receiving tokens.

## What a wallet actually stores

Here's a common misconception: **your crypto isn't stored "inside" the wallet app.** Your tokens live on the blockchain. What the wallet stores is your **private key** — the proof of ownership that lets you sign transactions and move your funds.

Think of it this way: your balance is on the blockchain ledger (visible to everyone), and your private key is the signature that proves the funds are yours to move.

## Types of wallets

| Type | Examples | Best for |
|------|----------|----------|
| **Social login (Web3Auth)** | Google, Apple sign-in | Beginners — no seed phrase needed |
| **Browser extension** | MetaMask, Phantom | Regular DeFi users |
| **Mobile wallet** | Trust Wallet, Phantom mobile | On-the-go trading |
| **Hardware wallet** | Ledger, Trezor | Large holdings, maximum security |

## Self-custody: "Not your keys, not your crypto"

You've probably heard this phrase. It means: **if you don't control the private key, you don't truly own the crypto.** When you hold tokens on a centralized exchange (like Coinbase or Binance), the exchange holds the keys — meaning they control your funds.

Self-custody — holding your own keys through a wallet you control — is a core principle of Gemach and the broader DeFi philosophy. It's what gives you true financial sovereignty.

{% hint style="warning" %}
**Never share your private key or seed phrase with anyone — ever.** No legitimate platform, support team, or person will ever ask for it. Anyone who does is trying to steal your funds. Write your seed phrase down on paper and store it somewhere safe offline — not in a screenshot, email, or cloud note.
{% endhint %}

## The easy option for beginners: Web3Auth on GDex Pro

Self-custody is important — but we also know that managing seed phrases can feel daunting for new users. That's why GDex Pro integrates **Web3Auth**, which lets you:

1. Sign in with your **Google or Apple account**
2. GDex Pro automatically creates a crypto wallet for you
3. Your wallet's private key is secured using advanced cryptography — split across multiple parties so that no single point of failure exists
4. You access your wallet just like any other app — with your social login

This gives you the benefits of self-custody (you control your funds) with the simplicity of a traditional app login.

<!-- Screenshot: The GDex Pro wallet connection screen showing the "Connect Wallet" button and Web3Auth social login options (Google, Apple) -->

{% hint style="success" %}
**Recommended for beginners:** Use Web3Auth on GDex Pro. Sign in with Google or Apple, and your wallet is created automatically — no seed phrase to worry about on day one. You can always export your keys and take full self-custody later when you're ready.
{% endhint %}

## For existing crypto users

If you already have MetaMask, Phantom, or another wallet, you can connect it directly on GDex Pro. The platform supports:

- **EVM wallets** (MetaMask, WalletConnect) for Ethereum, Base, BNB Chain, Arbitrum, Optimism
- **Phantom** for Solana
- **Sui-compatible wallets** for the Sui network

For a detailed walkthrough of wallet connection options, see the [GDex Pro Getting Started guide](../products/gdex-pro/getting-started-with-gdex.pro.md).

## Next step

Now you know what a wallet is and how to get one. Next, let's explore what you can actually **do** with your wallet in the world of DeFi. Continue to [🏦 What is DeFi?](what-is-defi.md)
