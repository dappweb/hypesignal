# HypeSignal 

> Social Sentient Analysis Crypto Trading Agent Powered by EigenAI and AgentKit

This is an AI-powered crypto trading agent that monitors Twitter/X influencers and automatically trades tokens based on positive sentiment analysis. Built with [Next.js](https://nextjs.org), [EigenAI](https://docs.eigencloud.xyz/products/eigenai/eigenai-overview), [AgentKit](https://github.com/coinbase/agentkit) and deployable on [EigenCompute](https://docs.eigencloud.xyz/products/eigencompute/eigencompute-overview)

## 🚀 Features

- **🐦 Twitter Monitoring**: Continuous monitoring of curated crypto influencers via twitterapi.io
- **🧠 Sentiment + Token Intelligence**: LLM-powered sentiment scoring and token extraction with project→ticker mapping
- **⚡ Automated Hyperliquid Execution**: Places IOC perp orders on Hyperliquid using your API wallet
- **🕒 Flexible Holding**: Positions stay open until you explicitly close/sell them (no more forced 24h liquidation)
- **🔄 Portfolio Sync**: Local DB holdings + live Hyperliquid account positions are merged and deduped in the dashboard
- **🍞 Real-Time Toasts**: Server-side trades/start-stop events surface instantly in the UI via live toast polling
- **🔒 AgentKit Tools**: Coinbase Developer Platform AgentKit remains available for onchain workflows

## 📦 Installation

1. **Install dependencies:**
```bash
npm install
```

2. **Set up environment variables:**
```bash
cp .env.example .env
```

3. **Configure your credentials in `.env`:**

### Required Environment Variables

```env
# EigenAI API Key (for AI processing)
EIGENAI_API_KEY=your_openai_api_key_here

# Coinbase Developer Platform credentials
CDP_API_KEY_ID=your_cdp_api_key_id
CDP_API_KEY_SECRET=your_cdp_api_key_secret
CDP_WALLET_SECRET=your_cdp_wallet_secret

# Twitter API Key (Twitterapi.io service)
TWITTER_API_KEY=your_twitter_api_key_here

# RPC URLs for different networks (optional - uses CDP defaults if not set)
BASE_MAINNET_RPC_URL=https://mainnet.base.org
BASE_TESTNET_RPC_URL=https://sepolia.base.org
ETHEREUM_MAINNET_RPC_URL=https://ethereum-rpc.publicnode.com
ETHEREUM_TESTNET_RPC_URL=https://ethereum-sepolia-rpc.publicnode.com

# Hyperliquid trading configuration
HYPERLIQUID_PRIVATE_KEY=0xyour_hyperliquid_api_wallet_key
HYPERLIQUID_ENVIRONMENT=testnet
# Leave blank or "*" to allow every market, otherwise comma-separated tickers
HYPERLIQUID_ALLOWED_MARKETS=
HYPERLIQUID_SLIPPAGE_BPS=50
HYPERLIQUID_TIME_IN_FORCE=Ioc
MAX_TRADE_AMOUNT_USD=30

# Trading configuration
TWEET_MAX_AGE_HOURS=6
```

### 🔑 Getting Your API Keys

#### Twitter/X API Setup
Visit [twitterapi.io](https://twitterapi.io)

#### Coinbase Developer Platform
1. Visit [CDP Portal](https://portal.cdp.coinbase.com/)
2. Create a new project
3. Generate API keys
4. Fund your wallet with test/real crypto

#### EigenAI API key
1. Visit [EigenAI](https://docs.eigencloud.xyz/products/eigenai/eigenai-overview)
2. Request access

#### EigenCompute Access
1. Visit [EigenCompute](https://docs.eigencloud.xyz/products/eigencompute/eigencompute-overview)
2. Request access

#### Hyperliquid API wallet
1. Visit the [Hyperliquid docs](https://hyperliquid.gitbook.io/hyperliquid-docs/for-developers/api/nonces-and-api-wallets) to create an API wallet
2. Generate a dedicated private key for API trading and fund it on Hyperliquid
3. Store the private key in `HYPERLIQUID_PRIVATE_KEY` and choose `HYPERLIQUID_ENVIRONMENT` (e.g., `testnet` for paper trading)

## Docker

Build the image and run the dev server inside a container.

```bash
# Build
docker build -t hypesignal:dev .

# Run (exposes http://localhost:3000)
docker run --rm -p 3000:3000 --env-file .env hypesignal:dev

# Hot-reload (mount local code into the container)
docker run --rm -it \
  -p 3000:3000 \
  --env-file .env \
  -v "$PWD":/app \
  hypesignal:dev
```

Notes:
- The image starts with `npm run dev` (Next.js dev server). The script removes `trading.db` on start.
- Do not bake secrets into images; pass via `--env-file` or `-e`.

## 🌐 What is EigenCompute?

[EigenCompute](https://docs.eigencloud.xyz/products/eigencompute/eigencompute-overview) is a **Verifiable Compute Layer** built on EigenLayer that enables decentralized, trust-minimized cloud computing with cryptographic proofs. It's the ideal platform for running AI trading agents like HypeSignal.

### Why EigenCompute for Trading Agents?

| Feature | Benefit for HypeSignal |
|---------|------------------------|
| **🔒 Verifiable Execution** | Cryptographic proofs ensure your trading logic executes exactly as intended—no operator tampering |
| **💰 Restaked Security** | Secured by EIGEN/ETH staking; dishonest operators face slashing penalties |
| **🛡️ TEE Isolation** | Trusted Execution Environments protect your API keys and trading strategies |
| **🔗 On-chain Composability** | Seamlessly integrate with DeFi protocols and smart contracts |
| **🐳 Docker Compatible** | Deploy existing containers with minimal changes |

### Key Features

1. **Off-chain, Verifiable Computation**
   - Execute AI workloads off-chain while providing cryptographic proofs of honest computation
   - Bridges Ethereum-grade trust with cloud-scale performance

2. **Hardware-based Security (TEEs)**
   - Code runs in isolated, tamper-proof Trusted Execution Environments
   - Attestation mechanisms prove computation ran as specified

3. **Economic Guarantees**
   - Operators stake collateral that gets slashed for malicious behavior
   - Aligns incentives for honest operation

4. **EigenAI Integration**
   - Deterministically verify AI model inference
   - Critical for trading applications where model calls must not be tampered with

### EigenCompute vs Traditional Cloud

| Aspect | Traditional Cloud | EigenCompute |
|--------|-------------------|--------------|
| Trust Model | Trust the provider | Cryptographically verifiable |
| Execution Proof | None ("black box") | Attestation + proofs |
| Security | Provider controls | TEE + staking guarantees |
| Transparency | Limited | Fully auditable |
| Ideal For | General apps | High-stakes AI/DeFi apps |

📚 **Learn More**: [EigenCompute Documentation](https://docs.eigencloud.xyz/products/eigencompute/eigencompute-overview)

---

## 🚀 How to Use EigenCompute

This section provides a complete guide to deploying HypeSignal on EigenCompute.

### Prerequisites

Before deploying, ensure you have:

| Requirement | Description |
|-------------|-------------|
| **Allowlisted Address** | Contact EigenLayer team to get your address allowlisted |
| **Docker** | Required to build and push container images |
| **Sepolia ETH** | For deployment transactions (get from [Google Cloud Faucet](https://cloud.google.com/application/web3/faucet/ethereum/sepolia) or [Alchemy Faucet](https://www.alchemy.com/faucets/ethereum-sepolia)) |
| **Environment Variables** | Your `.env` file with API keys configured |

### Step 1: Install EigenX CLI

**macOS/Linux:**
```bash
curl -fsSL https://eigenx-scripts.s3.us-east-1.amazonaws.com/install-eigenx.sh | bash
```

**Windows (PowerShell):**
```powershell
curl -fsSL https://eigenx-scripts.s3.us-east-1.amazonaws.com/install-eigenx.ps1 | powershell -
```

Verify installation:
```bash
eigenx --version
```

### Step 2: Authenticate

Generate a new keypair and store it locally:
```bash
eigenx auth generate --store
```

Or log in with an existing private key:
```bash
eigenx auth login
```

Check your authenticated address:
```bash
eigenx auth whoami
```

### Step 3: Prepare Your Environment

1. **Configure environment variables:**
```bash
cp .env.example .env
# Edit .env with your API keys
```

2. **Log into Docker:**
```bash
docker login
```

### Step 4: Deploy to EigenCompute

From your project directory with `Dockerfile` and `.env`:
```bash
eigenx app deploy
```

The CLI will guide you through:

```
Found Dockerfile in current directory.
? Choose deployment method:  [Use arrows to move, type to filter]
> Build and deploy from Dockerfile
  Deploy existing image from registry

? Enter image reference: [? for help] <image name>

App name selection:
? Enter app name: [? for help] (hypesignal)

? Do you want to view your app's logs?  [Use arrows to move, type to filter]
> Yes, but only viewable by me
  Yes, publicly viewable by anyone
  No, disable logs entirely

Building base image from Dockerfile...
#0 building with "desktop-linux" instance using docker driver
...
Your container will deploy with the following environment variables:

No public variables found

-----------------------------------------

PRIVATE VARIABLE          VALUE
----------------          -----
TWITTER_API_KEY           <key>
EIGENAI_API_KEY           <key>
HYPERLIQUID_PRIVATE_KEY   <key>
...

? Is this categorization correct? (y/N) y

Deploying new app...
App saved with name: hypesignal

App Name: hypesignal
App ID: <id>
Latest Release Time: <time>
Status: Deploying
IP: No IP assigned
EVM Address: <address>
Solana Address: <address>
```

### Step 5: Verify Deployment

Wait a few seconds, then check your app status:
```bash
eigenx app info hypesignal
```

Expected output when running:
```
App Name: hypesignal
App ID: <id>
Latest Release Time: <time>
Status: Running
IP: <ip>
EVM Address: <address>
Solana Address: <address>
```

🎉 **Congrats! Your agent is now running on EigenCompute!**

### Managing Your Deployment

| Command | Description |
|---------|-------------|
| `eigenx app list` | List all your deployed apps |
| `eigenx app info <name>` | Get details about a specific app |
| `eigenx app logs <name>` | View application logs |
| `eigenx app stop <name>` | Stop a running app |
| `eigenx app delete <name>` | Delete an app permanently |
| `eigenx app deploy` | Deploy or update an app |

### Accessing Your App

Once deployed, access your HypeSignal dashboard at:
```
http://<your-app-ip>:3000
```

Get your app's IP address with:
```bash
eigenx app info hypesignal
```

### Updating Your Deployment

To deploy updates:
```bash
# Make your code changes, then:
eigenx app deploy
```

The CLI will detect the existing app and prompt you to update it.

### Troubleshooting

| Issue | Solution |
|-------|----------|
| "Not allowlisted" | Contact EigenLayer team to allowlist your address |
| "Insufficient funds" | Get Sepolia ETH from a faucet |
| "Docker not found" | Install and start Docker Desktop |
| "Build failed" | Check your Dockerfile and run `docker build .` locally first |
| App not starting | Check logs with `eigenx app logs hypesignal` |

📚 **More Resources:**
- [EigenX CLI GitHub](https://github.com/Layr-Labs/eigenx-cli)
- [EigenCloud Forum](https://forum.eigenlayer.xyz/)
- [Building on EigenCloud Tutorial (YouTube)](https://www.youtube.com/watch?v=7x1NNbbg2TM)

---

## 🚀 Usage

1. Configure your influncer list at `config/trading.ts`.

2. **Start the development server:**
```bash
npm run dev
```

3. **Open [http://localhost:3000](http://localhost:3000)** in your browser

4. **Start the trading agent** using the UI controls or manually:
```bash
curl -X POST http://localhost:3000/api/trading/start
```

5. **Monitor positions** in the dashboard or via API:
```bash
curl http://localhost:3000/api/trading/status
```

## How It Works

### 1. Influencer Monitoring
The agent monitors tweets from your custom list of top crypto traders and influencers.

### 2. Sentiment Analysis
- Analyzes tweet content for positive crypto sentiment
- Looks for keywords like "bullish", "moon", "buy", "gem", etc.
- Filters out negative signals like "sell", "dump", "scam"

### 3. Token Detection
- Extracts token mentions from tweets ($BTC, #ethereum, etc.)
- Supports major cryptocurrencies
- Maps symbols to Hyperliquid markets using the SDK metadata

### 4. Automated Trading
- Places IOC limit buy orders on Hyperliquid when positive sentiment + token mention is detected
- Uses the configured USD notional per trade (see `MAX_TRADE_AMOUNT_USD`)
- Records fills and tweet context in SQLite

### 5. Position Tracking & Sync
- Persists every fill with tweet context, influencer handle, and profile image
- Merges Hyperliquid clearinghouse positions with the local DB to avoid duplicate buys
- Keeps positions open until you decide to sell (manually via SDK or Hyperliquid UI)

## ⚙️ Configuration

### Trading Parameters
Edit `config/trading.ts` to customize:

```typescript
export const TRADING_CONFIG = {
  influencers: [
    'blknoiz06',
    'trading_axe',
    'notthreadguy'
  ],
  maxTradeAmountUSD: 30,
  minimumConfidence: 70,
  tweetMaxAgeHours: 6,
  hyperliquid: {
    enabled: true,
    environment: 'testnet',
    allowedMarkets: [], // empty = allow any listed market
    slippageBps: 50,
    timeInForce: 'Ioc',
    symbolRouting: {
      WETH: 'ETH',
      ETH: 'ETH',
      SOL: 'SOL',
      BTC: 'BTC'
    }
  }
};
```

Tweak `allowedMarkets` to control which tickers can be traded—leave it empty (or set `HYPERLIQUID_ALLOWED_MARKETS=*` in `.env`) to allow every market—and extend `symbolRouting` when you want to translate tweet symbols into different Hyperliquid market names.

### Adding More Influencers
1. Open `config/trading.ts`
2. Add Twitter usernames to the `influencers` array
3. Restart the agent

## 🛠️ API Endpoints

- `POST /api/trading/start` - Start the trading agent
- `POST /api/trading/stop` - Stop the trading agent
- `GET /api/trading/status` - Get current status and positions
- `POST /api/agent` - Chat with the AgentKit (original functionality)

## 🗃️ Database Schema

The agent uses SQLite to track:

### Positions Table
- `id` - Unique position identifier
- `token` - Token symbol (BTC, ETH, etc.)
- `amount` - Filled size in base asset units
- `purchase_price` - Price when bought
- `purchase_time` - Timestamp of purchase
- `sell_time` - Timestamp of sale (if sold)
- `sell_price` - Price when sold
- `profit` - Calculated profit/loss
- `tweet` - Original tweet content
- `influencer` - Influencer who posted
- `profile_image_url` - Cached profile image for UI rendering
- `status` - holding/sold/failed

### Processed Tweets Table
- `tweet_id` - Twitter tweet ID
- `processed_at` - Timestamp processed

## ⚠️ Important Warnings

### Financial Risk
- **This is experimental software for educational purposes**
- **Never use with more money than you can afford to lose**
- **Cryptocurrency trading is extremely risky**
- **Past performance does not guarantee future results**
- **The bot may make losses**

### API Costs
- Twitter API Basic tier costs money (not free)
- OpenAI API usage incurs costs
- Gas fees for blockchain transactions

### Testing
- **Always test on testnets first (base-sepolia)**
- **Use small amounts initially**
- **Monitor the bot closely when running**

## 🐛 Troubleshooting

### Common Issues

**"Twitter stream error"**
- Check your Bearer Token is valid
- Ensure you have proper API access level
- Twitter API has rate limits

**"Failed to initialize agent"**
- Verify all environment variables are set
- Check CDP API credentials are correct
- Ensure wallet has sufficient balance

**"Token address not found"**
- Token may not be supported yet
- Add token address mapping in `lib/trading.ts`

### Debug Mode
Enable detailed logging by adding to `.env`:
```
NODE_ENV=development
```

## 📊 Monitoring

### Logs
- All trades are logged to console
- Database stores all positions
- Check browser console for errors

### Files Created
- `trading.db` - SQLite database
- `wallet_data.txt` - Wallet information (keep secure!)

## 🚀 Deployment

For production deployment:

1. Use `base-mainnet` network
2. Set strong environment variables
3. Use production API keys
4. Monitor resource usage
5. Set up proper logging
6. Consider using PostgreSQL instead of SQLite

---

## 📚 Learn More

- [AgentKit Documentation](https://docs.cdp.coinbase.com/agentkit/docs/welcome)
- [Coinbase Developer Platform](https://docs.cdp.coinbase.com/)
- [Twitter API v2 Docs](https://developer.x.com/en/docs/twitter-api)
- [Next.js Documentation](https://nextjs.org/docs)

## 🤝 Contributing

This project is built on top of AgentKit. For contributions:

- Fork this repository
- Make your changes
- Test thoroughly
- Submit a pull request

For AgentKit contributions:
- [AgentKit Contributing Guide](https://github.com/coinbase/agentkit/blob/main/CONTRIBUTING.md)
- [Join Discord](https://discord.gg/CDP)

## 📄 License

This project is for educational purposes. Please ensure you comply with:
- Twitter API Terms of Service
- OpenAI API Terms
- Coinbase Developer Platform Terms
- Local financial regulations
