---
description: Deep dive on the GDEX trading, copy-trading, and discovery skills — capabilities, real SDK method signatures, and the managed-custody auth flow
---

# 📈 GDEX Trading Skills

The trading skills give any agent full-stack DeFi capability through the `@gdexsdk/gdex-skill` package — spot swaps, HyperLiquid perpetual futures, limit orders, copy trading, bridging, portfolio analytics, and token discovery. Every example below uses the actual SDK method names and parameters from [`GemachDAO/gdex-skill`](https://github.com/GemachDAO/gdex-skill).

## Supported Chains

| Chain | ChainId | Native Token |
|-------|---------|--------------|
| Ethereum | `1` | ETH |
| Optimism | `10` | ETH |
| BNB Smart Chain | `56` | BNB |
| Sonic | `146` | S |
| Fraxtal | `252` | frxETH |
| Nibiru | `6900` | NIBI |
| Base | `8453` | ETH |
| Arbitrum One | `42161` | ETH |
| Berachain | `80094` | BERA |
| Solana | `622112261` | SOL |
| Sui | `1313131213` | SUI |
| HyperLiquid | perps only | USDC |

{% hint style="warning" %}
The Solana numeric chainId is **`622112261`** (`ChainId.SOLANA`), **not** `900`. Using `900` returns the EVM managed wallet with a `null` balance.
{% endhint %}

## Authentication

### Shared API keys (recommended for agents)

Two shared keys are pre-configured in the package, so agents can read data and submit managed trades without handling a user's private key:

```typescript
import { GdexSkill, GDEX_API_KEY_PRIMARY } from '@gdexsdk/gdex-skill';

const skill = new GdexSkill();
skill.loginWithApiKey(GDEX_API_KEY_PRIMARY);
skill.isAuthenticated(); // → true
```

Read-only endpoints (`getTrendingTokens`, `getTokenDetails`, `getOHLCV`, `getTopTraders`) require **no** authentication.

### Managed-custody session keys

All trading runs through **server-side managed wallets**. The user's control wallet signs **once** (the sign-in message); on-chain execution is performed by GDEX backend trade workers. Write operations use a secp256k1 **session keypair** plus AES-256-CBC encrypted `computedData`:

```typescript
import {
  GdexSkill,
  GDEX_API_KEY_PRIMARY,
  generateGdexSessionKeyPair,
  buildGdexSignInMessage,
  buildGdexSignInComputedData,
} from '@gdexsdk/gdex-skill';

const skill = new GdexSkill();
skill.loginWithApiKey(GDEX_API_KEY_PRIMARY);
const apiKey = GDEX_API_KEY_PRIMARY;

// 1. Generate a session keypair
const { sessionPrivateKey, sessionKey } = generateGdexSessionKeyPair();

// 2. The control wallet signs the sign-in message (EIP-191 personal_sign)
const userId = '0xYourControlWallet';
const nonce = String(Math.floor(Date.now() / 1000) + Math.floor(Math.random() * 1000));
const message = buildGdexSignInMessage(userId, nonce, sessionKey);
const signature = /* wallet.signMessage(message) */ '';

// 3. Submit the encrypted sign-in payload
const signIn = buildGdexSignInComputedData({ apiKey, userId, sessionKey, nonce, signature });
await skill.signInWithComputedData({ computedData: signIn.computedData, chainId: 622112261 });
```

{% hint style="info" %}
The encryption is **deterministic** AES-256-CBC: the key/IV are derived from `SHA256(apiKey)` — never use a random IV or the `iv:ciphertext` format. Sign-in is the only operation that uses EIP-191; all post-sign-in operations sign with the raw `keccak256` + secp256k1 session key (`v` = raw recovery param `00`/`01`). The **gdex-authentication** skill documents every helper.
{% endhint %}

---

## Skill Reference

### `gdex-spot-trading`

Buy and sell tokens across Solana, Sui, and EVM chains with automatic DEX routing.

```typescript
// Buy with native token (0.1 SOL)
const trade = await skill.buyToken({
  chain: 'solana',
  tokenAddress: 'EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v', // USDC
  amount: '0.1',
  slippage: 1,        // optional, default 1%
  dex: 'raydium',     // optional: force a DEX
});
// → { jobId, status, inputAmount, outputAmount, txHash?, error? }

// Sell an absolute amount
await skill.sellToken({ chain: 8453, tokenAddress: '0x...', amount: '100', slippage: 0.5 });

// Sell a percentage of holdings
await skill.sellToken({ chain: 'solana', tokenAddress: '...', amount: '50%' });
```

**Example prompts:** "Buy 0.1 SOL of USDC", "Sell 50% of my BONK", "Buy $50 of ARB on Arbitrum".

---

### `gdex-perp-trading`

HyperLiquid perpetual futures — open/close positions, set leverage, place market and limit orders with TP/SL.

```typescript
// Open a 10x BTC long with TP/SL
const pos = await skill.openPerpPosition({
  coin: 'BTC',
  side: 'long',
  sizeUsd: '1000',
  leverage: 10,
  takeProfitPrice: '110000',
  stopLossPrice: '95000',
  marginMode: 'cross',   // 'cross' (default) | 'isolated'
});

await skill.closePerpPosition({ coin: 'BTC' });                   // close 100%
await skill.closePerpPosition({ coin: 'ETH', closePercent: 50 }); // close 50%
await skill.setPerpLeverage({ coin: 'BTC', leverage: 20 });

const positions = await skill.getPerpPositions({ walletAddress: '0x...' });
```

**Example prompts:** "Open a 5x long on ETH with $200", "Close half my BTC short", "Set BTC leverage to 20x".

---

### `gdex-perp-funding`

Deposit and withdraw USDC to/from HyperLiquid (human-readable amounts, converted internally; 10 USDC minimum deposit).

```typescript
await skill.perpDeposit({ amount: '10' });  // deposit 10 USDC
await skill.perpWithdraw({ amount: '5' });   // withdraw 5 USDC
```

**Example prompts:** "Deposit 25 USDC to HyperLiquid", "Withdraw 10 USDC from my perp account".

---

### `gdex-limit-orders`

Create, update/delete, and list limit orders via managed-custody encrypted payloads. Endpoints are `limit_buy` / `limit_sell` / `update_order` / `orders`.

```typescript
// Limit buy with optional TP/SL
await skill.limitBuy({
  apiKey: GDEX_API_KEY_PRIMARY,
  userId: '0xControlWallet',
  sessionPrivateKey,
  chainId: 622112261,
  tokenAddress: 'EKpQGSJtjMFqKZ9KQanSqYXRcF8fBopzLHYxdM65zcjm',
  amount: '10000000',        // lamports
  triggerPrice: '0.50',
  profitPercent: '50',       // optional TP
  lossPercent: '25',         // optional SL
});

// Delete an order (isDelete: true)
await skill.updateOrder({
  apiKey: GDEX_API_KEY_PRIMARY, userId: '0xControlWallet', sessionPrivateKey,
  chainId: 622112261, orderId: '<64-hex>', isDelete: true,
});

// List active orders (session-key auth)
const { count, orders } = await skill.getLimitOrders({ userId, data: encryptedSessionKey, chainId: 622112261 });
```

**Example prompts:** "Buy TOKEN when it drops to $0.50", "Set a take-profit sell at $999", "Cancel my open limit order".

---

### `gdex-copy-trading`

Discover top wallets and create/manage **Solana** copy trades. Discovery is read-only; writes are Solana-only and sign in with `chainId: 622112261`.

```typescript
// Discovery (no auth)
const topWallets = await skill.getCopyTradeWallets();      // top 300 by total PnL
const gems = await skill.getCopyTradeGems();               // hot new tokens from top wallets

// Create a copy trade (computedData auth)
await skill.createCopyTrade({
  apiKey: GDEX_API_KEY_PRIMARY,
  userId: controlWalletAddress,
  sessionPrivateKey,
  chainId: 622112261,
  traderWallet: 'SolanaTraderAddress',
  copyTradeName: 'Alpha Trader',
  buyMode: 1,                 // 1 = fixed SOL, 2 = percentage
  copyBuyAmount: '0.001',
  lossPercent: '50',
  profitPercent: '100',
  copySell: true,
});
```

{% hint style="warning" %}
There is no pause/toggle on the backend — both `isDelete: true` and `isChangeStatus: true` **permanently delete** a copy trade.
{% endhint %}

**Example prompts:** "Show the top Solana wallets by PnL", "Copy 0xLeader with 0.001 SOL per trade", "Delete my Alpha Trader copy trade".

---

### `gdex-perp-copy-trading`

Copy HyperLiquid perpetual-futures traders. Completely separate from Solana copy trading; sign-in uses `chainId: 1` and all ABI fields are strings.

```typescript
// Discovery (no auth)
const topByVolume = await skill.getHlTopTraders('volume');
const topByPnl = await skill.getHlTopTradersByPnl();
const stats = await skill.getHlUserStats('0xManagedWalletAddress'); // managed wallet, not control

// Create an HL copy trade (computedData auth)
await skill.createHlCopyTrade({
  apiKey, userId: controlWalletAddress, sessionPrivateKey,
  traderWallet: '0xTraderEvmAddress',
  copyTradeName: 'BTC Whale',
  copyMode: 1,                       // 1 = fixed USD, 2 = proportion
  fixedAmountCostPerOrder: '50',
  lossPercent: '25',                 // mandatory, 0 < x < 100
  profitPercent: '100',              // mandatory, > 0
  oppositeCopy: false,               // true = copy the opposite direction
});
```

**Example prompts:** "Who are the top HyperLiquid traders this week?", "Copy 0xWhale's perps at $50 per order", "Copy 0xTrader but inverted".

---

### `gdex-bridge`

Cross-chain bridging of native tokens between EVM chains, Solana, and Sui (via ChangeNow). Always quote first, then execute.

```typescript
import { ChainId } from '@gdexsdk/gdex-skill';

const quote = await skill.getBridgeQuote({
  fromChain: 'solana',
  toChain: ChainId.ETHEREUM,
  tokenAddress: 'EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v',
  amount: '100',
});
console.log(quote.outputAmount, quote.feeUsd);

const result = await skill.bridge({
  fromChain: 'solana',
  toChain: ChainId.ETHEREUM,
  tokenAddress: 'EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v',
  amount: '100',
  destinationAddress: '0xYourEthAddress',
  slippage: 0.5,
});
```

**Example prompts:** "Bridge 100 USDC from Solana to Ethereum", "What does it cost to bridge 0.5 ETH to Arbitrum?".

---

### `gdex-portfolio`

Cross-chain portfolio overview, chain-specific balances, and paginated trade history.

```typescript
import { ChainId } from '@gdexsdk/gdex-skill';

const portfolio = await skill.getPortfolio({ walletAddress: '0x...' });
// → { totalValueUsd, balances, perpPositions?, realizedPnl, unrealizedPnl }

const balances = await skill.getBalances({ walletAddress: '0x...', chain: ChainId.ETHEREUM });

const history = await skill.getTradeHistory({ walletAddress: '0x...', page: 1, limit: 20 });
```

**Example prompts:** "Show my portfolio across all chains", "What's my ETH balance on Base?", "List my last 20 trades".

---

### `gdex-token-discovery`

Token details, trending tokens, OHLCV candles, and top traders.

```typescript
const token = await skill.getTokenDetails({ tokenAddress: '...', chain: 'solana' });
// → { symbol, name, priceUsd, priceChange24h, marketCap, fdv, volume24h, liquidity }

const trending = await skill.getTrendingTokens({ chain: 'solana', period: '24h', limit: 20, minLiquidity: 50000 });

const ohlcv = await skill.getOHLCV({
  tokenAddress: 'So11111111111111111111111111111111111111112',
  chain: 'solana',
  resolution: '60', // '1'|'5'|'15'|'30'|'60'|'240'|'D'|'W'
  from: Math.floor(Date.now() / 1000) - 86400,
  to: Math.floor(Date.now() / 1000),
});

const traders = await skill.getTopTraders({ chain: 'solana', period: '7d', limit: 10, sortBy: 'pnl' });
```

**Example prompts:** "What's trending on Solana?", "Get the 1h candles for SOL", "Show the top traders this week".

---

### `gdex-livestream-discovery`

Solana livestream-token discovery — currently-live streams, per-token live status, and big-buy alert feeds. Useful for surfacing tokens with active community livestreams before momentum trades.

**Example prompts:** "Which Solana tokens are livestreaming right now?", "Is this token live?", "Show me the big-buy alert feed".

---

## Error Handling

The SDK throws typed errors so agents can recover gracefully:

```typescript
import {
  GdexAuthError,        // 401/403 — re-authenticate
  GdexValidationError,  // invalid params (.field)
  GdexApiError,         // 4xx/5xx (.statusCode)
  GdexNetworkError,     // connection failures, timeouts
  GdexRateLimitError,   // 429 — has .retryAfter (seconds)
} from '@gdexsdk/gdex-skill';
```

When something goes wrong, hand off to **gdex-sdk-debugging**, which maps error codes, encryption issues, and chain-specific gotchas to fixes.

---

**See also:**
- [🛠️ Platform & UI Skills](utility-skills.md) — onboarding, transfers, social, HyperLiquid extras, React UI, and debugging
- [✏️ Creating Custom Skills](creating-custom-skills.md) — build your own trading skill
- [🧩 Agent Skills Overview](README.md) — the complete catalog and install options
