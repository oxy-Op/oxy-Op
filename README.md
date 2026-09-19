# Oxy — Solana / Backend Engineer

**Rust / TypeScript · Solana programs · transaction infrastructure · indexers · high-throughput backends**

I build the systems underneath on-chain products — programs, SDKs, transaction builders, indexers, trading/lending infrastructure, and data systems.

I started programming in 2019 and moved into Solana around 2023. TypeScript/Node.js is my main product/backend stack; I use Rust for on-chain programs and performance-critical infrastructure.

I’ve worked across full product lifecycles: architecture, protocol design, backend APIs, transaction construction, indexing, deployment, monitoring, and production debugging.

I use AI tooling heavily for implementation speed, but I own the architecture, debugging, testing, and correctness of what I ship.

---

## Production work

### Beezie — Solana Collectibles Infrastructure

Worked across the core Solana stack behind **Beezie Claw, Marketplace, and Drop**.

Built:
- Solana programs
- TypeScript SDKs
- non-custodial transaction-building backends
- finalized event/indexer foundations
- Metaplex Core integrations

The systems covered marketplace listings, bids, escrow settlement, royalties, NFT commit/reveal flows, drops, buybacks, custody/transfer logic, transaction lifecycle handling, and on-chain event ingestion.

I worked alongside frontend/backend engineers as these systems were integrated into the wider Beezie product.

---

### Glyde — Solana Trading Infrastructure

Built transaction infrastructure for a Solana trading product across **10+ DEX / launch venues**, including Raydium, Meteora, Pump.fun-style markets, bonding curves, and AMMs.

The transaction service included:
- metadata-driven transaction building to remove RPC calls from the hot path
- fallback state fetching when metadata was unavailable
- v0 transaction construction
- buy/sell simulation against real Solana RPC
- balance-delta / slippage / fee validation
- per-DEX health simulation
- RPC fallback, retries, rate limits, monitoring and alerting
- Docker + GitHub Actions + blue/green deployments

Also built a real-time token holder-count service in Rust using **Helius Laserstream/gRPC + Redis**, decoding SPL Token / Token-2022 account updates directly and maintaining holder counts through atomic zero-crossing updates instead of repeated `getProgramAccounts` scans.

---

### SendIt — Lending & Margin

Built product-specific Solana infrastructure around a modified **Solend-derived lending protocol**.

My work included:
- custom two-reserve market and risk constraints
- protocol-enforced collateral / borrowing roles
- cumulative reward-index / reward-debt liquidity incentives
- fee vesting into lender liquidity
- adapted TypeScript SDK and account parsers
- non-custodial lending transaction APIs
- permissionless market-creation transaction orchestration
- automated liquidation infrastructure

Built an **atomic margin engine** combining protocol flash loans, Jupiter swaps, lending deposits/borrows, oracle updates, compute-budget instructions and LUT-backed v0 transactions.

Also built historical + realtime indexing that reconstructed positions from chain activity, including weighted entry/exit prices, partial closes, realized/unrealized PnL, and reconciliation of deposits, withdrawals and liquidations performed outside the normal UI flow.

---

### Limbo — Solana Staking

Built the Solana staking program and its backend integration.

The program supported configurable staking limits and lock periods, PDA vault custody and multiple independent positions per user.

Built a TypeScript/Apollo GraphQL layer around the program with:
- queries and mutations
- transaction construction
- account derivation
- GraphQL/WebSocket subscriptions
- realtime program-event handling
- position and TVL queries

Implemented cumulative reward accounting using a global reward index + per-user reward debt rather than iterating over stakers.

---

### Meyland — Game / Real-Estate Backend

Built the initial backend versions for a wallet-enabled game / real-estate platform before later development was continued by the wider team.

Work included:
- Express + GraphQL / REST
- Prisma + MongoDB
- wallet authentication
- JWT/RSA verification
- role-based authorization
- profiles and wallet history
- daily streak / reward systems
- building and quest progression
- external integrations and application-level staking logic

---

## Systems I'm building

### High-performance Solana historical RPC / data engine

Building a Rust historical RPC engine around a **custom storage/database layer** rather than putting Solana history behind a conventional SQL database.

Work includes:
- custom binary storage
- `io_uring` / direct-I/O oriented reads
- 4 KiB-oriented storage access
- custom account/signature indexes
- multi-epoch query routing
- low-allocation / zero-copy response paths
- base58/base64 serialization optimization
- cache and NVMe behavior analysis
- extensive A/B performance and correctness benchmarking

Current qualified GTFA workloads are in the **3K+ RPS range at 100 transactions per response** on commodity server hardware, with substantially higher throughput on lighter RPC methods.

This project is deliberately benchmark-driven: optimizations only survive when response semantics remain identical.

---

### High-throughput Solana parser / indexer

Building a Rust transaction parsing and indexing engine for turning raw Solana execution data into normalized protocol facts.

Work includes:
- raw transaction / instruction parsing
- Borsh decoding
- CPI and inner-instruction attribution
- Anchor and native program events
- protocol-specific normalization
- typed trade / liquidity / state facts
- wallet and protocol activity reconstruction
- ClickHouse analytical projections
- streaming ingestion
- replay / failure handling
- explorer, PnL, OHLCV and wallet-analysis use cases

The goal is to parse Solana directly rather than depend on third-party enriched APIs for product data.

---

## Selected independent work

### Anigacha
Native **Pinocchio** Solana NFT lottery experiment using Switchboard VRF, Metaplex Core custody, zero-copy/chunked accounts, refund handling and grind-resistant mechanics. Deployed on devnet.

### Pinocchio → IDL
Tooling for reproducible IDL generation for native Pinocchio programs. Annotate Rust source and generate Codama JSON plus Anchor/Solscan-compatible IDLs, with CI checks that fail when generated interfaces drift.

### Kaoru
Open-source Discord anime card-collection framework with character/image ingestion, summons, trading, fusion, economy systems and a Next.js administration panel.

### DevPilot
MCP-based VPS automation tool that provisions fresh servers over SSH — Node.js, PM2, Rust, Nginx, Redis, reverse proxies, SSL and GitHub CI/CD/deployment setup.

### SolAbs
Experimental Solana social-login wallet / account-abstraction concept with automatic wallet creation, encrypted key storage, WebAuthn, Prisma, Next.js APIs and payment → SOL onboarding flows.

---

<details>
<summary><strong>More things I've built</strong></summary>

<br />

**Solana / DeFi**
- Meteora Alpha Vault automation with scheduled FIFO deposits, eligibility checks, permissioned-vault proofs, restart persistence and state monitoring
- Wavr music-token launchpad integrations using Meteora DBC, Birdeye and Supabase
- Solana escrow contracts for wagered two-player games
- NFT management tooling for Metaplex Core, compressed NFTs and Candy Machine flows
- token creation / mint / burn / authority / holder-snapshot / airdrop tooling
- early Raydium AMM trading/sniping automation
- informal Solana program security reviews and architecture audits
- token migration + staking systems supporting roughly **$1M TVL**

**Trading / Data**
- Python Binance trading client with order placement, position management, TP/SL and trailing-stop flows
- wallet portfolio / PnL / net-worth analysis using Birdeye
- multi-chain market-data integrations using Codex GraphQL and CoinMarketCap
- custom Solana portfolio / transaction parsing including Jupiter, Raydium and Meteora activity

**Backend / Infrastructure**
- AWS ECS/ECR + GitHub Actions deployment-security work with OIDC/IAM, review gates, reproducible artifacts and multisig-oriented Solana program deployment workflows
- Docker / DigitalOcean / PostgreSQL operational ownership for inherited production services
- realtime and restart-safe Solana event listeners / microservices
- Redis, PostgreSQL, MongoDB, ClickHouse, GraphQL, REST and gRPC systems

**AI / ML / Security experiments**
- trained YOLO models on self-collected and annotated game footage; built a two-machine realtime inference + ATmega32U4 HID hardware pipeline
- ElizaOS-based agents with X / Telegram integrations and generated media
- speaker/headphone bidirectionality security POC
- mouse acoustic side-channel / FFT experiment
- keyboard acoustic side-channel ML experiments using MFCC / mel features and multiple classifiers

</details>

---

## Stack

**Core:** Rust · TypeScript · Node.js · Python

**Solana:** Anchor · Pinocchio · Web3.js · Solana Kit · SPL Token / Token-2022 · Metaplex Core · Jupiter · Raydium · Meteora · Pyth · Switchboard

**Data / Backend:** PostgreSQL · MongoDB · Redis · ClickHouse · Prisma · GraphQL · REST · gRPC / Yellowstone / Laserstream

**Infrastructure:** Docker · GitHub Actions · AWS ECS/ECR · DigitalOcean · Nginx · PM2 · Linux

**Frontend:** React · Next.js · TanStack Query · Zustand

---

I particularly enjoy work where correctness and performance both matter: transaction construction, protocol state, indexing, low-latency systems, storage engines, and infrastructure that has to survive real production traffic.

📫 **Telegram:** [@OxKaoru](https://t.me/OxKaoru)  
🌐 **Website:** [kaoru7.com](https://kaoru7.com)
