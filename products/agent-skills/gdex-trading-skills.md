---
description: Deep dive on all 14 GDEX trading skills — capabilities, example prompts, supported chains, and the authentication flow
---

# 📈 GDEX Trading Skills

The 14 GDEX trading skills give any Gemach agent full-stack DeFi capability via the `@gdexsdk/gdex-skill` package. From simple spot buys to cross-chain bridging to perpetual futures with copy trading — these skills cover the complete trading lifecycle.

## Supported Chains

All GDEX trading skills operate across the following networks:

| Chain | Chain ID |
|-------|----------|
| Solana | 622112261 |
| Ethereum | 1 |
| Base | 8453 |
| Arbitrum | 42161 |
| BNB Smart Chain | 56 |
| Optimism | 10 |
| Sui | (native) |
| Sonic | (native) |
| Berachain | (native) |

## Authentication Flow

Most trading skills require an active GDEX session before executing transactions. Authentication uses a **session key pair** model:

```typescript
import {
  generateGdexSessionKeyPair,
  buildGdexSignInMessage,
  buildGdexSignInComputedData,
} from '@gdexsdk/gdex-skill';
import { ethers } from 'ethers';

// 1. Generate a disposable session key pair
const sessionKeyPair = generateGdexSessionKeyPair();

// 2. Build the sign-in message with your wallet address
const message = buildGdexSignInMessage({
  walletAddress: '0xYourWalletAddress',
  sessionPublicKey: sessionKeyPair.publicKey,
});

// 3. Sign the message with your wallet
const wallet = new ethers.Wallet(privateKey);
const signature = await wallet.signMessage(message);

// 4. Build computed authentication data
const computedData = buildGdexSignInComputedData({
  message,
  signature,
  sessionKeyPair,
});
```

The session key pair is ephemeral — it authorizes the agent to act on behalf of your wallet for the duration of the session without exposing your private key.

---

## Skill Reference

### `gdex-authentication`

**Purpose:** Manage the full GDEX authentication lifecycle — generating session key pairs, constructing sign-in messages, and maintaining session state.

**Key capabilities:**
- Generate session key pairs for wallet-delegated signing
- Build and sign GDEX authentication messages
- Refresh expired sessions automatically
- Store session state securely for use by other skills

**Example prompts:**
```
"Authenticate my wallet 0x1234...abcd with GDEX."
"My session expired — re-authenticate and retry the trade."
"Set up GDEX auth using my Solana wallet."
```

**Prerequisite for:** All transaction-executing skills (`gdex-spot-trading`, `gdex-perp-trading`, `gdex-bridge`, etc.)

---

### `gdex-wallet-setup`

**Purpose:** Configure and register wallets for use across all GDEX trading skills.

**Key capabilities:**
- Register EVM wallets (MetaMask, hardware wallets, raw private keys)
- Register Solana wallets (Phantom, raw keypairs)
- Set a default wallet for agent-initiated transactions
- Validate wallet connectivity and balance checks

**Example prompts:**
```
"Set up my Base wallet ending in 0xABCD for GDEX trading."
"Add my Solana wallet and make it the default."
"Check that my wallet is properly configured for GDEX."
```

---

### `gdex-onboarding`

**Purpose:** Guide first-time users through the complete GDEX setup process.

**Key capabilities:**
- Walk through wallet creation or import
- Set up GDEX API key configuration
- Fund wallet with Apple Pay, bank transfer, or on-chain deposit
- Verify end-to-end connectivity before first trade

**MCP Tools exposed (available to IDE agents via MCP):**

| Tool | Description |
|------|-------------|
| `search_gdex_docs` | Search GDEX documentation for any topic |
| `get_sdk_pattern` | Retrieve TypeScript code patterns for SDK operations |
| `get_api_info` | Look up API endpoint details and parameters |
| `explain_workflow` | Get step-by-step explanations of GDEX workflows |
| `get_chain_info` | Retrieve chain IDs, RPC endpoints, and network details |
| `get_trading_guide` | Spot and limit order trading guidance |
| `get_copy_trade_guide` | Copy trading setup and configuration |
| `get_component_guide` | SDK component usage and integration patterns |

**Example prompts:**
```
"I'm new to GDEX — walk me through getting set up."
"Help me onboard and make my first trade."
"What do I need before I can start trading?"
```

---

### `gdex-spot-trading`

**Purpose:** Execute spot buy and sell orders across all supported chains.

**Key capabilities:**
- Market buys and sells for any token by address or symbol
- Configurable slippage tolerance
- Multi-chain support (Solana, Ethereum, Base, BNB Chain, Arbitrum, Optimism, and more)
- Transaction confirmation with receipt parsing

**Core SDK functions:**

```typescript
import { buyToken, sellToken } from '@gdexsdk/gdex-skill';

// Buy 0.1 SOL worth of a token
const buyResult = await buyToken({
  chainId: 622112261,           // Solana
  tokenAddress: 'mint-address',
  amountInSol: 0.1,
  slippageBps: 100,             // 1% slippage
  sessionKeyPair,
});

// Sell 50% of a token holding
const sellResult = await sellToken({
  chainId: 622112261,
  tokenAddress: 'mint-address',
  percentageToSell: 50,
  slippageBps: 150,
  sessionKeyPair,
});
```

**Example prompts:**
```
"Buy $50 worth of BONK on Solana."
"Sell 25% of my ETH position on Base."
"Buy 0.01 ETH of ARB token on Arbitrum."
```

---

### `gdex-limit-orders`

**Purpose:** Place limit buy and sell orders with optional take-profit and stop-loss automation.

**Key capabilities:**
- Set entry price for automated buy execution
- Set exit price with take-profit targets
- Trailing stop-loss configuration
- View and cancel open orders

**Core SDK functions:**

```typescript
import { limitBuy, limitSell } from '@gdexsdk/gdex-skill';

// Place a limit buy at 10% below current price
const order = await limitBuy({
  chainId: 8453,                 // Base
  tokenAddress: '0xtoken...',
  amountUsd: 100,
  limitPriceUsd: 1.80,
  sessionKeyPair,
});

// Place a limit sell with take-profit
const sellOrder = await limitSell({
  chainId: 8453,
  tokenAddress: '0xtoken...',
  percentageToSell: 100,
  limitPriceUsd: 2.50,
  sessionKeyPair,
});
```

**Example prompts:**
```
"Set a limit buy for 100 USDC of TOKEN at $1.80."
"Place a take-profit sell order at $2.50 for my full position."
"Show me all my open limit orders."
"Cancel the limit buy I set earlier."
```

---

### `gdex-token-discovery`

**Purpose:** Find trending, new, and high-potential tokens with security and volume analysis.

**Key capabilities:**
- Scan trending tokens across all supported chains
- Filter by minimum security score, volume threshold, and holder count
- Get token metadata: market cap, 24h volume, holder distribution, contract audit status
- Identify newly launched tokens in configurable time windows

**Core SDK functions:**

```typescript
import { getTrendingTokens, getNewTokens, getTokenInfo } from '@gdexsdk/gdex-skill';

// Get top 10 trending tokens on Base with at least 70% security score
const trending = await getTrendingTokens({
  chainId: 8453,
  limit: 10,
  minSecurityScore: 70,
});

// Get tokens launched in the last 24 hours on Solana
const newTokens = await getNewTokens({
  chainId: 622112261,
  hoursAgo: 24,
  minVolume: 10000,
});
```

**Example prompts:**
```
"What are the top trending tokens on Solana right now?"
"Find new tokens launched today on Base with at least $50k volume."
"Give me a security analysis of token address 0x1234..."
"What tokens are trending across all chains?"
```

---

### `gdex-portfolio`

**Purpose:** Track holdings, aggregate balances, and calculate P&L across all wallets and chains.

**Key capabilities:**
- Aggregate token balances across multiple wallets and chains
- Real-time USD value calculation
- P&L tracking with cost-basis history
- Exportable portfolio snapshots
- Alert thresholds for significant value changes

**Example prompts:**
```
"Show me my full portfolio across all chains."
"What's my total P&L this week?"
"How much is my Solana wallet worth in USD?"
"Alert me if my portfolio drops more than 10% in a day."
```

---

### `gdex-bridge`

**Purpose:** Transfer assets cross-chain between any supported GDEX network.

**Key capabilities:**
- Estimate bridge fees and time before committing
- Execute bridge transactions with status tracking
- Support for all major bridge routes (Solana ↔ EVM, EVM ↔ EVM)
- Transaction receipt parsing with destination chain confirmation

**Core SDK functions:**

```typescript
import { estimateBridge, requestBridge } from '@gdexsdk/gdex-skill';

// Estimate cost before bridging
const estimate = await estimateBridge({
  fromChainId: 1,               // Ethereum
  toChainId: 8453,              // Base
  tokenAddress: '0xusdc...',
  amount: '100000000',          // 100 USDC (6 decimals)
  sessionKeyPair,
});
// Returns: BridgeEstimate { fee: string, estimatedTimeSeconds: number, route: string }

// Execute the bridge
const bridgeResult = await requestBridge({
  fromChainId: 1,
  toChainId: 8453,
  tokenAddress: '0xusdc...',
  amount: '100000000',
  destinationAddress: '0xReceiver...',
  sessionKeyPair,
});
// Returns: BridgeResult { txHash: string, orderId: string, status: string }
```

**Bridge response types:**

```typescript
interface BridgeEstimate {
  fee: string;                  // Fee in source token
  feeUsd: string;               // Fee in USD
  estimatedTimeSeconds: number;
  route: string;                // Bridge protocol used
  minimumReceived: string;
}

interface BridgeResult {
  txHash: string;
  orderId: string;
  status: 'pending' | 'processing' | 'complete' | 'failed';
  destinationTxHash?: string;
}
```

**Example prompts:**
```
"Bridge 100 USDC from Ethereum to Base."
"How much does it cost to bridge 0.5 ETH to Arbitrum?"
"Transfer all my USDC from Base to Solana."
"What's the status of my bridge transaction 0xabc...?"
```

---

### `gdex-perp-trading`

**Purpose:** Open, manage, and close leveraged perpetual futures positions on HyperLiquid.

**Key capabilities:**
- Market and limit entry for long/short positions
- Leverage configuration (1x–50x depending on asset)
- Position management: add margin, partial close, full close
- Real-time P&L and liquidation price monitoring

**Example prompts:**
```
"Open a 5x long on ETH-PERP with $200."
"Close half my BTC short position."
"What are my current open perp positions?"
"Set a stop-loss on my SOL-PERP long at $150."
```

---

### `gdex-perp-funding`

**Purpose:** Monitor and manage perpetual funding rate exposure across all open positions.

**Key capabilities:**
- Real-time funding rate monitoring for all open positions
- Funding rate history and trend analysis
- Alert on high funding rate conditions
- Automatic position flip when funding becomes severely negative

**Example prompts:**
```
"What's the current funding rate on my ETH-PERP long?"
"Alert me if funding on any position exceeds 0.05%."
"Show me funding rates across all perpetual markets."
```

---

### `gdex-copy-trading`

**Purpose:** Mirror the spot trading activity of identified high-performing wallets.

**Key capabilities:**
- Search for and evaluate top-performing wallets by win rate and P&L
- Configure copy parameters: position size scaling, token allowlist/blocklist
- Monitor copy performance vs. source wallet
- Pause or stop copying individual traders

**Example prompts:**
```
"Find the top 5 wallets by win rate on Solana this month."
"Copy trade wallet 0xLeader... with 10% of their position sizes."
"Show me how my copy trade performance compares to the source wallet."
"Stop copying 0xTrader... but keep the existing positions."
```

---

### `gdex-perp-copy-trading`

**Purpose:** Mirror the perpetual futures strategies of top HyperLiquid traders.

**Key capabilities:**
- Identify top HyperLiquid traders by ROI and Sharpe ratio
- Copy both entries and exits with configurable lag
- Risk management: max position size, max leverage override
- Independent pause/stop per copied trader

**Example prompts:**
```
"Who are the top-performing perp traders on HyperLiquid this week?"
"Copy trade 0xPerpTrader... on HyperLiquid perps with 50% size scaling."
"What's the P&L on my perp copy trades this month?"
```

---

### `gdex-trading`

**Purpose:** High-level trading orchestration skill that coordinates other trading skills for complex, multi-step strategies.

**Key capabilities:**
- Execute compound strategies that span multiple skills (e.g., discover → analyze → buy → set limit sell)
- Manage strategy state across multiple trades
- Schedule and automate recurring trading actions
- Aggregate results from multiple trading sub-skills into unified reports

**Example prompts:**
```
"Find trending tokens on Solana, buy the top 3, and set 2x take-profit sells."
"Every hour, check my portfolio and rebalance if any position exceeds 30%."
"Run my momentum strategy: buy tokens up 20% in 4 hours with volume > $100k."
```

---

### `gdex-sdk-debugging`

**Purpose:** Diagnose and resolve GDEX SDK integration issues, failed transactions, and API errors.

**Key capabilities:**
- Decode GDEX SDK error codes with plain-language explanations
- Trace failed transaction causes (insufficient balance, slippage, RPC error, etc.)
- Check API connectivity and rate limit status
- Suggest fixes for common integration issues

**Example prompts:**
```
"My buy transaction failed with ERR_SLIPPAGE — what should I do?"
"Debug why my bridge transaction is stuck in 'pending'."
"The SDK is throwing a 401 — help me fix my auth."
```

---

## How Skills Interact with the GDEX SDK

Each GDEX trading skill wraps the `@gdexsdk/gdex-skill` npm package. The SDK provides TypeScript functions that the skill's helper scripts invoke; the skill's `SKILL.md` instructions tell the agent when and how to call those helpers.

The interaction chain:

```
Agent receives user prompt
       ↓
Skill instructions loaded into context
       ↓
Agent identifies appropriate tool
       ↓
Tool calls helper script (Node.js / shell)
       ↓
Helper script calls @gdexsdk/gdex-skill
       ↓
SDK executes on-chain transaction
       ↓
Helper returns JSON result to agent
       ↓
Agent formats response for user
```

Installing the SDK:

```bash
npm install @gdexsdk/gdex-skill
# or
yarn add @gdexsdk/gdex-skill
```

---

**See also:**
- [🛠️ Utility Skills](utility-skills.md) — the 8 non-trading skills
- [✏️ Creating Custom Skills](creating-custom-skills.md) — build your own trading skill
- [🧩 Agent Skills Overview](README.md) — the complete skill catalog
