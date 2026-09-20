# Oxy — Solana / Backend Engineer

**Rust · TypeScript · Solana programs · transaction infrastructure · indexers · backend-first full-stack**

I build the systems underneath on-chain products — programs, SDKs, transaction builders,
indexers, trading/lending infrastructure, data systems, and the application backends around them.

I started programming in 2019 and moved into Solana around 2023. TypeScript/Node.js is my
main product/backend stack; I use Rust for on-chain programs and performance-critical systems,
and I've spent plenty of time in Next.js/React when a product needs frontend work.

I like owning things end-to-end: architecture, implementation, integration, deployment,
monitoring, debugging, and figuring out which guarantees belong on-chain vs. off-chain.

---

# Selected Production Work

## [Beezie](https://solana.beezie.com/)

**Tokenized collectibles platform combining physical vaulting, digital ownership, Claw-style
pulls, instant liquidity and a peer-to-peer marketplace. At its Solana expansion in May 2026,
Beezie reported $142M+ ARR, $100M+ Base volume and 540K+ claw pulls.**

I worked on the Solana infrastructure across **Claw, Marketplace and Drop**.

- Built the on-chain programs, TypeScript SDKs, transaction-building services and indexing
  foundations used to integrate the products into the wider application.
- Designed settlement and lifecycle flows around Metaplex Core assets: custody, listings,
  bids, escrow, royalties, purchases, commit/reveal, drops and buybacks.
- Worked closely with the frontend/backend team as the systems moved from protocol design
  through application integration and production handoff.

<details>
<summary><strong>Technical details</strong></summary>

<br />

**Stack:** Rust · Anchor · TypeScript · Solana Web3.js / Kit · Metaplex Core · SPL Token ·
Fastify · v0 transactions · Anchor event CPI

### Claw
- commit/reveal settlement
- program-controlled Core asset custody and transfer
- seed-commit verification
- machine/inventory accounting
- discount state
- reveal/cranker transaction construction
- SDK instruction builders, PDA helpers and event decoders
- event/indexer foundations with deduplication and historical backfill

### Marketplace
- listings, listing updates and cancellation
- peer-to-peer purchases
- bids backed by canonical token escrow
- bid cancellation / acceptance
- creator royalty settlement
- Core transfer-delegate flows
- stale-listing cleanup with deliberately restricted operator authority

### Drop
- purchase → reveal → buyback lifecycle
- program/SDK for permanent purchase receipts and asset settlement
- transaction-only backend for admin/public builders
- v0 transaction construction and signer boundaries
- finalized event persistence
- historical backfill, reconciliation and restart-safe indexer state


</details>

---

## [Glyde](https://www.gizmolab.io/case-studies/glyde)

**Solana trading infrastructure providing a normalized transaction-construction layer across
10+ DEX, AMM, launch and bonding-curve integrations.**

- Built buy/sell transaction infrastructure with a metadata-driven production path that avoids
  unnecessary RPC calls during construction, plus an on-chain state-fetch fallback.
- Built simulation-driven correctness checks that construct, sign and simulate transactions
  against Solana before production use, validating balance changes, fees and slippage.
- Built a live holder-count service in Rust using SPL / Token-2022 account streams and atomic
  state updates instead of repeatedly scanning every token account with `getProgramAccounts`.

<details>
<summary><strong>Technical details</strong></summary>

<br />

**Transaction infrastructure:** TypeScript · Node.js · Fastify · Solana Web3.js · Jest ·
Raydium · Meteora · Pump.fun-style venues

Supported transaction paths included Raydium AMM/CPMM, Meteora DLMM/DAMM/DBC, Pump-style
markets and other venue-specific builders.

The service included:
- metadata-first transaction construction
- RPC fallback paths
- real `simulateTransaction` integration tests
- synthetic per-DEX health checks that exercised the complete build/simulate path
- RPC monitoring and fallback
- categorized error tracking
- rate limits and request validation
- Gatus monitoring / status surfaces
- Docker + GitHub Actions deployment
- blue/green rollout and rollback handling

**Live holder count:** Rust · Helius Laserstream/gRPC · Redis · Lua

Subscribed directly to SPL Token and Token-2022 account changes, decoded only the account
fields required for the product, tracked write versions and atomically adjusted holder counts
when an account crossed `0 ↔ non-zero`.

This replaced an expensive RPC-wide polling approach used previously.

</details>

---

## [SendIt](https://x.com/senditfun)

**Solana lending + leveraged-trading product built around a heavily customized
Solend/SPL-lending-derived foundation.**

- Extended the lending system with product-specific market topology, risk constraints,
  reward-index incentives and fee-derived lender yield
- Built a non-custodial transaction backend that prepared lending and market-creation flows
  while leaving final signing with the user's wallet.
- Built an atomic margin engine composing flash loans, Jupiter swaps, lending operations,
  oracle updates and fee settlement into leveraged open/close transactions.
- Built chain-derived position indexing and liquidation infrastructure rather than relying
  solely on API-side bookkeeping.

<details>
<summary><strong>Technical details</strong></summary>

<br />

**On-chain:** Rust · Solana · SPL Token · modified Solend lending state/instructions

Product-specific program work included:
- enforced two-reserve market structure
- protocol-controlled collateral vs. borrowable reserve roles
- custom LTV / liquidation policies
- market finalization rules
- cumulative reward-index / reward-debt supplier incentives
- overlapping finite-duration reward campaigns
- borrow-fee vesting progressively released into reserve liquidity

**Backend:** TypeScript · Express · Web3.js · Jupiter · Pyth · Switchboard · Redis ·
Prisma / MongoDB

The margin engine handled:
- flash borrow
- Jupiter swap
- collateral deposit
- protocol borrow
- platform fee
- atomic flash repayment
- v0 transactions
- Address Lookup Tables
- compute-budget / priority-fee instructions
- oracle update transactions
- setup / cleanup token accounts

**Indexer / liquidation:**
- historical backfill + realtime finalized logs
- persistent slot checkpoints
- Jupiter event decoding
- weighted entry/exit prices
- realized and unrealized PnL
- partial closes
- reconciliation of direct deposits, withdrawals and liquidations
- off-chain obligation health calculation
- automated liquidation + collateral redemption

The base reserve/obligation/interest/liquidation primitives originated from spl-library;
the product-specific protocol, backend, indexing and operational work above was the custom layer.

</details>

---

## [Limbo / Youmio](https://limbo.youmio.ai/)

**Solana staking product with configurable lock positions and a realtime backend used by the
application.**

- Built the staking program and backend integration end-to-end, including program-controlled
  custody, multiple independent positions per user and configurable lock/stake constraints.
- Used cumulative reward-index / reward-debt accounting so rewards could accrue without
  iterating over every staker.
- Built the transaction/read/event layer consumed by the application, including realtime
  subscriptions for program activity.

<details>
<summary><strong>Technical details</strong></summary>

<br />

**Stack:** Rust · Anchor · TypeScript · Node.js · Apollo GraphQL · Express · WebSockets ·
Solana Web3.js

Program work included:
- global configuration
- minimum / maximum stake rules
- configurable lock ranges
- PDA vault custody
- position IDs
- multiple positions per wallet
- stake / unstake / config flows
- account cleanup

Backend included:
- GraphQL queries and mutations
- transaction construction
- PDA/account derivation
- TVL/config queries
- position-specific account reads
- GraphQL subscriptions over WebSockets
- realtime program-event handling

</details>

---

## [Meyland](https://mey.network/)

**Wallet-enabled game / real-estate platform combining player progression, property/building
mechanics and Web3 account integration.**

- Built the initial backend foundation before later development was continued by the wider team.
- Owned authentication, user/account state, wallet integration, progression/reward mechanics,
  external-service integrations and the APIs consumed by the product.
- Kept game/application state in the backend where appropriate rather than forcing ordinary
  product logic on-chain.

<details>
<summary><strong>Technical details</strong></summary>

<br />

**Stack:** TypeScript · Express · GraphQL / REST · Prisma · MongoDB · Dynamic.xyz · JWT/RSA

Included:
- wallet authentication
- JWT verification using RSA public keys
- role-based access control
- user profiles
- wallet history
- metadata / progression
- daily streak and reward systems
- building progression
- quests
- email / external-service integrations
- application-level staking logic

</details>

---

# Current Infrastructure Work

## Solana Historical RPC / Data Engine — Private

**Building a high-performance Rust historical RPC system backed by a custom on-disk data engine
rather than a conventional database.**

- Work spans custom storage/indexing, `io_uring` / direct-I/O style access, 4 KiB-oriented
  reads, multi-epoch querying and low-allocation response paths.
- Performance work is regression-gated against real historical data: an optimization only
  survives if output semantics remain correct.
- Selected heavy `getTransactionsForAddress` workloads serve **thousands of 100-transaction
  pages per second** on commodity server hardware.

<details>
<summary><strong>Technical details</strong></summary>

<br />

**Stack:** Rust · Axum · io_uring · O_DIRECT · mmap · ZSTD · NVMe

Work includes:
- custom historical transaction storage
- account and signature indexes
- multi-epoch routing / pagination
- lazy epoch loading
- direct I/O
- small aligned read patterns
- low-allocation / zero-copy-style response construction
- JSON / base58 / base64 serialization optimization
- cache behavior analysis
- compression / block geometry experiments
- NVMe and CPU profiling
- reproducible correctness/performance benchmarks

</details>

---

## Solana Parser / Analytics Engine — Private

**Building a high-throughput Rust parsing/indexing stack that turns raw Solana execution data
into normalized protocol facts for trading, wallet, PnL and market-data products.**

- Parses raw instructions, inner instructions, CPI and Borsh-encoded protocol data rather than
  depending on third-party enriched transaction APIs.
- Normalizes heterogeneous protocol activity into typed facts that can drive wallet activity,
  market state, PnL, OHLCV and analytical projections.
- Uses ClickHouse projections for downstream analytical workloads.

<details>
<summary><strong>Technical details</strong></summary>

<br />

**Stack:** Rust · Borsh · Yellowstone / gRPC · ClickHouse

Work includes:
- raw transaction conversion
- instruction parsing
- event parsing
- CPI / inner-instruction attribution
- Borsh decoding
- protocol-specific normalization
- typed swap / liquidity / state facts
- wallet and protocol activity reconstruction
- ClickHouse-backed analytical projections
- replay / failure handling
- explorer-style transaction views
- OHLCV / PnL / wallet-analysis foundations

Private parser-versioning and IDL strategy is intentionally omitted.

</details>

---

# More Work

<details>
<summary><strong>Open a much larger list of things I've built</strong></summary>

<br />

## Professional / Freelance

### TMPL — Token Migration + Staking
Built an atomic Solana V1 → V2 token migration program, backend/integration layer and later
a staking product for the V2 token.

The system handled roughly **$1M TVL at peak**, giving me meaningful responsibility over
real production value very early in my professional Solana work.

**Tech:** Rust · Anchor · TypeScript · Solana Web3.js · Raydium-era integrations

---

### Molly Network — QR / Claim Infrastructure
Built backend infrastructure for a QR-driven product where physical beverage QR codes led
users to wallet verification and token/NFT reward claims.

Also built a separate claim listener microservice that tracked token/NFT claims and updated
application state.

**Tech:** TypeScript · Solana Web3.js · Express · Prisma · event listeners

---

### Wavr
Worked on a music-based token launchpad with Meteora DBC token creation / swaps, backend
database logic and market-data integration.

**Tech:** TypeScript · Solana · Meteora DBC · Supabase · Birdeye

---

### Lvl
Built a deliberately minimal Solana escrow/custody program for two-player games.

The program handled:
- stake funding
- atomic creation / joining
- payout
- timeout refunds
- lifecycle events

Game logic intentionally stayed off-chain.

**Tech:** Rust · Anchor · TypeScript

---

### Meteora Alpha Vault Automation
Built automation for participating when Meteora Alpha Vault deposit windows opened.

Included:
- single / multi-pool deposits
- scheduled execution
- eligibility checks
- permissioned vault / Merkle-proof handling
- state monitoring
- persistence across restarts
- operation logging

---

### Solana Program Auditing
Performed program reviews ranging from smaller NFT-minting contracts to a larger
investment/revenue-sharing protocol.

Review work covered areas such as:
- account / authority validation
- redundant signature verification
- CPI / compute design
- state invariants
- instruction architecture
- transaction-size / LUT tradeoffs
- economic and settlement flows

One NFT review found no critical vulnerability but surfaced architectural and compute
inefficiencies.

---

### Solana Release Security / OpSec
Two-month engagement focused on making program deployment and release processes safer.

Built / worked on:
- GitHub Actions CI/CD
- approval / review gates
- build-version verification
- reproducible release artifacts
- multisig program-upgrade packaging / signing flows
- AWS ECS / ECR build infrastructure
- self-hosted runners
- OIDC
- IAM controls

The goal was to make it difficult for an unreviewed or mismatched program binary to reach
production.

---

### Telegram Game Backend / Infrastructure
Helped stabilize an inherited game backend after previous developers had left.

Worked across:
- existing game/economy backend fixes
- Docker lifecycle
- DigitalOcean deployment
- PostgreSQL operations
- secure PostgreSQL administration over an SSH tunnel

Project name intentionally omitted.

---

### Iris — Agentic AI
Worked with ElizaOS-style agents integrating:
- X posting
- generated media
- Telegram
- token / market / project-discovery information

---

## Earlier / Independent Solana Work

### Solana Tools Suite
Built a Next.js + TypeScript + Solana tooling application covering:
- token creation
- mint / burn
- airdrops
- closing token accounts
- authority revocation
- holder snapshots
- compressed asset/token airdrop integrations

Also wrote small Solana programs/CPI flows around parts of the tooling.

---

### ViraLabs
Early professional Solana work included:
- Solana staking backend using Web3.js
- claims
- NFT staking
- giveaways
- TypeScript backend work
- Next.js interfaces

Also partially built a broader multi-chain claims/staking/raffle product.

---

### NFT Tooling
Built tooling around:
- CSV → Metaplex Core minting
- compressed NFTs
- Candy Machine
- bulk asset workflows

---

### Early Trading Automation
Built:
- Python Binance trading client using orderbook/trading APIs
- TP / SL / trailing-stop / position-management flows
- early Raydium AMM sniper automation targeting newly active pools

These were some of my first trading/infrastructure projects around 2023.

---

### EVM / viem
Worked with ABI-driven EVM interactions for NFT allowlists and related contract flows.

---

### Wallet Analyzer
Built a Birdeye-backed wallet analysis tool for portfolio value, PnL, holdings and protocol
usage classification.

Included:
- API throttling / rate limiting
- SQLite persistence
- wallet activity classification
- small reporting UI

This is intentionally distinct from my lower-level parser/indexer work above — this project
used an external enriched-data provider.

---

### Market Data Integrations
Built integrations around Codex GraphQL, CoinMarketCap and multi-chain token data for
market-cap / volume / category filtering and product displays.

---

## Open Source / Self Projects

### [Anigacha](https://github.com/oxy-Op/anigacha)
Native Pinocchio-based Solana NFT lottery experiment.

**Tech / ideas:** Pinocchio · Switchboard VRF · Metaplex Core custody · zero-copy /
chunked accounts · refund handling · grind-resistance mechanics

Deployed on devnet.

---

### [Pinocchio → IDL](https://github.com/oxy-Op/pinocchio-to-idl)
Tooling for reproducible interface generation for native Pinocchio programs.

Annotate Rust source once and generate:
- Codama JSON
- Anchor-compatible IDLs
- Solscan-compatible interfaces
- CI checks that fail when generated interfaces drift

---

### [Kaoru](https://github.com/oxy-Op/kaoru-card-bot)
Open-source Discord anime card-collection framework inspired by collection/economy games.

Included:
- character/image ingestion
- summons / pulls
- trading
- fusion
- economy systems
- Next.js administration panel

---

### [DevPilot](https://github.com/oxy-Op/DevPilot)
MCP-based VPS automation tool for initializing and managing fresh servers over SSH.

Automated:
- Node.js
- PM2
- Rust
- Nginx
- Redis
- reverse proxy / domain setup
- SSL
- GitHub deploy keys
- CI/CD workflows

---

### [SolAbs](https://github.com/oxy-Op/solabs)
Experimental social-login / wallet-onboarding proof of concept for Solana.

Explored:
- social authentication
- automatic wallet creation
- encrypted key storage
- WebAuthn
- Prisma
- Next.js APIs
- payment → SOL onboarding

This was a concept project, not production custody infrastructure.

---

### Community Protocol Suite
Large experimental Anchor project started around 2024 with separate programs for:
- staking
- SPL claims
- raffles
- giveaways
- access / fee control

A root community program coordinated subordinate programs through CPI.

The partial backend experimented with GraphQL and queue-based infrastructure.

The project was discontinued and is not presented as production-audited software.

</details>

---

# Experiments

## Realtime Computer Vision → Hardware Control

Built a two-machine computer-vision/hardware experiment from scratch around realtime game footage.

- Captured and annotated my own dataset using CVAT / Roboflow and trained YOLO models for
  player/head detection.
- Desktop captured gameplay frames; a second machine performed inference and sent control
  commands over UART/USB to an **ATmega32U4 / Leonardo-class microcontroller** acting as a
  USB HID mouse.
- Also experimented with movement logic, latency, physical hardware integration and soldering.

The goal was a research/fun POC around how far an external CV + HID pipeline could go.
A significant amount of low-level Arduino/control implementation was AI-assisted; I don't
claim embedded specialization from it.

<details>
<summary><strong>Other security / signal-processing experiments</strong></summary>

<br />

- **Keyboard acoustic side channel:** recorded keystroke samples, extracted MFCC/delta/
  mel-spectrogram features and experimented with RF/SVM/GBT/CNN classifiers.
- **Mouse acoustic side channel:** explored whether high-polling-rate optical mouse sensors
  can detect desk vibration / acoustic frequencies, including FFT analysis.
- **SPEAKE(a)R-style POC:** explored published research around speakers/headphones behaving
  bidirectionally and audio jack retasking.

These were curiosity-driven proof-of-concepts and did not reach a clean production-quality
end-to-end result.

</details>

---

# Stack

**Rust · TypeScript / Node.js · Python · Anchor · Pinocchio · Next.js / React ·
Solana Web3.js / Kit · Metaplex Core · Jupiter · Raydium · Meteora ·
gRPC / Yellowstone · PostgreSQL · Redis · ClickHouse · MongoDB · GraphQL ·
Docker · GitHub Actions · AWS**

---

I use AI tooling heavily where it accelerates implementation, but I still own the architecture,
debugging, testing, integration and production correctness of what I ship.

📫 **Telegram:** [@OxKaoru](https://t.me/OxKaoru)
