# Hackathon Resources: Stellar Ecosystem

A curated navigation guide for Stellar ecosystem resources, gotchas, starter code, and reference implementations. Organized by when and why you'd reach for each resource.

---

## 1. Environment and API Setup

Everything you need before writing a line of code. For the full reference (API keys, verified contract addresses, auth patterns, and setup gotchas), see `Dev_Setup_Guide.md`.

### Testnet Quick Reference

| Resource | Value |
|---|---|
| Soroban RPC (testnet) | `https://soroban-testnet.stellar.org` |
| Horizon (testnet) | `https://horizon-testnet.stellar.org` |
| Etherfuse sandbox | `https://sandbox.etherfuse.com` |
| Friendbot (fund testnet accounts) | `https://friendbot.stellar.org` |
| Testnet USDC issuer (Circle) | `GBBD47IF6LWK7P7MDEVSCWR7DPUWV3NY3DTQEVFL4NAT4AQH3ZLLFLA5` |
| Testnet USDC issuer (Blend/Etherfuse) | `GATALTGTWIOT6BUDBCZM3Q4OQ4BO2COLOAZ7IYSKPLC2PMSOPPGF5V56` |

### API Keys to Get Early

- **Etherfuse sandbox key:** https://etherfuse.com (developer signup)
- **Soroswap/DeFindex:** no key needed on testnet
- **BlindPay / AlfredPay:** check their respective docs for sandbox access

### Things Worth Knowing Upfront

- Testnet USDC has two different issuers depending on which protocol you're using; they don't share liquidity. Check which one your protocol expects before swapping.
- Etherfuse: your `customer_id` is permanent. Generate it once, store it, reuse it.
- Stellar memos max out at 28 bytes. Anything longer silently fails.
- SDK v14 renamed several core types. If copying older examples, check the migration notes.

### Deeper Reference

| Resource | What it is | Link |
|---|---|---|
| **Stellar Hackathon FAQ** (briwylde08) | Community-compiled FAQ from building with anchors and DeFi protocols | https://github.com/briwylde08/stellar-hackathon-faq |
| **Stellar DeFi Gotchas** (kaankacar) | 400+ findings from 60 vibe-coding runs; organized by protocol | https://github.com/kaankacar/stellar-defi-gotchas |
| **Arroz Wallet field report** (rice2000) | Full build log: Python/Flask wallet with Etherfuse on/off-ramp, 14 hours of real development | https://gist.github.com/rice2000/f5cba666112b4fa69afbc70891c47782 |
| **DevRel Experiment** (kaankacar) | Controlled comparison of Claude Code with vs. without resources, including full test results | https://github.com/kaankacar/stellar-devrel-experiment |

---

## 2. Regional Starter Pack (Mexico + Latin America)

The fastest path to a working integration with Mexican peso rails.

**Repo:** https://github.com/ElliotFriend/regional-starter-pack

A SvelteKit app paired with a portable TypeScript anchor library containing three anchor integrations. The key insight: `/src/lib/anchors/` is framework-agnostic; copy it into any TypeScript or Node project and it works without the SvelteKit wrapper.

### Anchor Providers Included

| Anchor | What it does |
|---|---|
| **Etherfuse** | MXN to CETES (Mexican government bonds) via SPEI. Mexico-first. |
| **AlfredPay** | MXN to USDC via SPEI |
| **BlindPay** | Global, MXN to USDB via SPEI |

### SEP Protocols Implemented

SEP-1, SEP-10, SEP-6, SEP-12, SEP-24, SEP-31, SEP-38

### Bonus

Includes pre-configured MCP servers for Claude Code (Svelte docs + Etherfuse docs), so Claude understands the stack out of the box.

---

## 3. Reference Implementations

### Stellar DeFi App

Full mainnet DeFi dashboard: https://github.com/kaankacar/stellar-defi-app

Integrates Blend, Soroswap, Phoenix, Aquarius, SDEX, and Reflector Oracle in a single working application. The most useful thing about this repo is seeing how all the protocols compose: what the actual API calls look like, what the response shapes are, and how to wire health factors and price feeds together.

| Protocol | What it does |
|---|---|
| Blend v1 | Lending/borrowing with health factor monitoring |
| Soroswap Aggregator | DEX swap routing across multiple protocols |
| Phoenix DEX | Token swaps |
| Aquarius AMM | Token swaps |
| SDEX | Native Stellar DEX |
| Reflector Oracle | On-chain USD price feeds |

---

## 4. Ecosystem Discovery

| Resource | What it is | Link |
|---|---|---|
| **Stellar Ecosystem Resources** (stellar) | Workshop activations, reference guides for Soroban, wallet integration, DeFi protocols, OpenZeppelin, tokens, and security | https://github.com/stellar/ecosystem-resources/ |
| **Stellar Ecosystem DB** (lumenloop) | Structured YAML database of 646 Stellar projects with metadata: categories, funding, contracts, GitHub/social links | https://github.com/lumenloop/stellar-ecosystem-db |
| **Awesome Stellar Community Fund** (lumenloop) | 9 AI skills for SCF grant applications + full guides on what gets funded. Useful post-hackathon if applying for SCF. | https://github.com/lumenloop/awesome-stellar-community-fund |

**How to use the Ecosystem DB:** Find existing projects in your category to avoid reinventing the wheel, discover integration partners, check on-chain contract addresses.

---

## 5. Videos Worth Watching

From the Build on Stellar channel and Kaan Kacar (Senior Developer Advocate at SDF).

| Title | Description | Link |
|---|---|---|
| **Scoping and Evaluating Your Project** | How to evaluate and validate your idea before building. A must-watch before writing any code. | https://x.com/BuildOnStellar/status/2029288908639666402 |
| **A First Look: Nethermind's SPP** | Stellar Private Payments (ZK-based privacy layer on Stellar) | https://x.com/i/broadcasts/1pKkOyOyOnwKj |
| **Vibe Coding 5 ZK Games in 90 Minutes** | Live demo using 5 different AI models to build 5 ZK games in 90 minutes | https://x.com/i/broadcasts/1dRKZaYYbbdxB |
| **The Builder's Guide to AI Prompt Engineering** | How to write better prompts for AI-assisted development | https://x.com/BuildOnStellar/status/2008902190090613013 |
