---
description: The non-trading GDEX skills — onboarding, auth, wallet setup, transfers, social, HyperLiquid extras, promotion, React UI generation, and SDK debugging
---

# 🛠️ Platform & UI Skills

Beyond the [trading skills](gdex-trading-skills.md), `@gdexsdk/gdex-skill` ships a set of skills that handle onboarding, account setup, transfers, social/community features, HyperLiquid extras, partner promotion, full React UI generation, and debugging. Together they let an agent take a user from "no wallet" all the way to a deployed trading dashboard.

## Getting Started Skills

### `gdex-onboarding`

The **start-here** skill. Gives the agent a GDEX overview — architecture, the 12 supported chains, the full list of available skills, and a quickstart for managed-custody trading. Agents load this first to learn how the rest of the skills fit together.

**Example prompts:** "I'm new to GDEX, walk me through it", "What can you do?", "How do I make my first trade?".

### `gdex-authentication`

Manages the full managed-custody auth lifecycle — shared API-key login, session-keypair generation, AES-256-CBC `computedData` encryption, and secp256k1 trade signing. It documents every helper (`generateGdexSessionKeyPair`, `buildGdexSignInMessage`, `buildGdexManagedTradeComputedData`, …) used by the trading skills.

{% hint style="info" %}
Encryption is **deterministic** AES-256-CBC keyed from `SHA256(apiKey)`. Sign-in uses EIP-191; every post-sign-in operation signs with the raw `keccak256` + secp256k1 session key. See the auth flow in [GDEX Trading Skills](gdex-trading-skills.md#authentication).
{% endhint %}

**Example prompts:** "Authenticate my wallet with GDEX", "My session expired — re-authenticate", "Sign in with my Solana wallet".

### `gdex-wallet-setup`

For users **without** a wallet. Generates an EVM **control wallet** offline (keys never leave the machine), creates a session keypair, and fetches wallet info. After sign-in, the backend automatically provisions the matching Solana and other chain-specific trading wallets server-side.

```typescript
import { generateEvmWallet, generateGdexSessionKeyPair } from '@gdexsdk/gdex-skill';

const wallet = generateEvmWallet();                 // { address, privateKey, mnemonic }
const { sessionPrivateKey, sessionKey } = generateGdexSessionKeyPair();
```

**Example prompts:** "Create a wallet for me", "Set up a new GDEX control wallet", "Generate a session key".

---

## Transfers & Social Skills

### `gdex-transfers`

Native and ERC-20/SPL token transfers via GDEX managed custody — send assets to any recipient on a supported chain.

**Example prompts:** "Send 10 USDC to 0xRecipient on Base", "Transfer 0.5 SOL to this address".

### `gdex-watchlist-social`

The community layer around individual tokens — manage a watchlist, post comments, and cast sentiment (bullish/bearish) votes.

**Example prompts:** "Add this token to my watchlist", "What's the sentiment on BONK?", "Vote bullish on this token".

### `gdex-token-import`

Import user-defined custom tokens so they appear in token details, balances, and the portfolio across the platform — useful for long-tail or freshly launched tokens the indexer hasn't picked up yet.

**Example prompts:** "Import this token address", "Add my custom token to the portfolio".

---

## HyperLiquid Extras

### `gdex-hl-outcomes`

HyperLiquid **HIP-3 outcome / event (prediction) markets** — list markets, place outcome orders, and manage outcome positions. Outcome coins use the `#<outcomeId><sideIndex>` format; the skill can also fetch 24h volume per market over the HyperLiquid WebSocket.

**Example prompts:** "List the HyperLiquid event markets", "Buy YES on the NBA Finals market", "Show my outcome positions".

### `gdex-hl-referral`

HyperLiquid referral information and reward claims — distinct from the HL copy-trading referral flows.

**Example prompts:** "What are my HyperLiquid referral rewards?", "Claim my HL referral rewards".

---

## Promotion & Partner Skills

### `gdex-trending-promotion`

Paid trending-slot promotion — book featured slots on the trending feed and check booking status.

**Example prompts:** "Promote this token in the trending feed", "Is my trending slot active?".

### `gdex-retailer-onboarding`

Lists branded onboarding partners that **white-label** the GDEX trading stack — for retailers integrating GDEX into their own products.

**Example prompts:** "Which retailers white-label GDEX?", "Show the partner onboarding options".

---

## Frontend (React / Next.js) Skills

These six skills generate production-ready React/Next.js UI that wraps the SDK. They share the official Gemach brand tokens (Rich Black `#060A17`, Majorelle `#704FF6`, Argentinian `#61B8FF`) with a dark-mode default.

| Skill | What it generates |
|-------|-------------------|
| `gdex-ui-install-setup` | Project setup — SDK initialization, context providers, environment variables, TypeScript config |
| `gdex-ui-trading-components` | Order entry forms, position tables, copy-trade panels, orderbook displays, PnL views |
| `gdex-ui-portfolio-dashboard` | Portfolio dashboards — token balances, trade history, chain selectors, live data polling |
| `gdex-ui-wallet-connection` | Connect buttons, account displays, chain switching, API-key vs wallet auth UI flows |
| `gdex-ui-theming` | CSS theming — dark/light modes, trading colors, responsive breakpoints, Tailwind config |
| `gdex-ui-page-layouts` | Full page compositions — trading, portfolio, copy-trading, and bridge pages |

**Example prompts:** "Scaffold a Next.js app wired to the GDEX SDK", "Build a portfolio dashboard component", "Add a wallet connect button", "Theme my trading UI with the Gemach palette", "Generate a full trading page layout".

---

## Developer Tools

### `gdex-sdk-debugging`

Troubleshoot GDEX SDK errors — a reference for error codes, encryption debugging, chain-specific quirks, HyperLiquid gotchas, and copy-trade pitfalls. The agent reaches for this skill whenever a call throws.

Common error classes:

| Class | When thrown |
|-------|-------------|
| `GdexAuthError` | 401/403, invalid credentials — re-authenticate |
| `GdexValidationError` | Invalid address, amount, chain, or slippage |
| `GdexApiError` | Non-success HTTP (4xx/5xx) |
| `GdexNetworkError` | Connection refused, timeout |
| `GdexRateLimitError` | HTTP 429 — has `.retryAfter` in seconds |

**Example prompts:** "My buy failed with a slippage error — what now?", "Debug why my bridge is stuck in pending", "The SDK is throwing a 401, help me fix auth".

---

## Composing Skills

The platform skills are designed to compose with the trading skills into multi-step workflows:

| Combination | Workflow |
|-------------|----------|
| `gdex-wallet-setup` → `gdex-authentication` → `gdex-spot-trading` | Onboard a brand-new user and place their first trade |
| `gdex-token-discovery` → `gdex-watchlist-social` | Discover trending tokens and add the best to a watchlist |
| `gdex-token-import` → `gdex-portfolio` | Import a long-tail token so it shows up in the portfolio |
| `gdex-ui-install-setup` → `gdex-ui-page-layouts` | Scaffold an app and drop in a full trading page |
| any trading skill → `gdex-sdk-debugging` | Recover from a failed call |

---

**See also:**
- [📈 GDEX Trading Skills](gdex-trading-skills.md) — spot, perps, copy trading, bridge, portfolio, discovery
- [✏️ Creating Custom Skills](creating-custom-skills.md) — add your own skill
- [🧩 Agent Skills Overview](README.md) — the full catalog and install options
