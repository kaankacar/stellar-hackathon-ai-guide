# Stellar Hackathon AI Guide

Mexico has one of the most active SPEI networks in the world, a massive remittance corridor with the US, and millions of people who move money through channels that are slow, expensive, or both. Stellar is built for exactly this, and this hackathon is a chance to build something that actually matters for people here.

This repo is a collection of guides put together by the SDF DevRel team to help you move fast. The goal is simple: every developer at this event should have the same shot at building something real, regardless of whether they have a paid AI subscription, years of Stellar experience, or a high-end laptop.

## Start here

**Building with Mexican peso rails?** The regional starter pack (`Hackathon_Resources.md`) is the fastest path. It has Etherfuse (MXN to CETES via SPEI), AlfredPay (MXN to USDC via SPEI), and BlindPay already wired up as a portable TypeScript library you can drop into any Node project. Before you use Etherfuse, read its section in `Dev_Setup_Guide.md`. There are several non-obvious gotchas (auth format, customer_id permanence, sandbox simulation) that have cost developers hours.

**Don't have a paid AI subscription?** Start with `Free_AI_Setup.md`. It covers six capable open-source models, how to run them locally for free via Ollama, and how to connect them to Claude Code. If your laptop can't run a large model, it also covers renting a GPU server for a few dollars on RunPod or Vast.ai.

**About to write code?** Read `Dev_Setup_Guide.md` first. Five minutes here saves hours later.

## Suggested reading order

1. `Free_AI_Setup.md` if you need a free AI setup
2. `Dev_Setup_Guide.md` before writing any code
3. `Hackathon_Resources.md` to orient yourself in the Stellar ecosystem
4. `Claude_Code_Guide.md` if you're using Claude Code
5. `Recommended_AI_Tools.md` to explore what else is available

## Dev_Setup_Guide.md

Everything you need before writing code, in one place.

**API keys:** Etherfuse and DeFindex have no self-service signup. You have to contact the team (stablebond@etherfuse.com for Etherfuse, Discord: paltalabs for DeFindex) and budget 24-48h for a response. Get this done before the hackathon starts. Soroswap, Phoenix, Aquarius, and Blend require no key.

**Testnet contract addresses:** Verified DeFindex addresses are included (Factory, USDC vault, XLM vault). Addresses for Soroswap, Phoenix, Blend, Aquarius, and Reflector Oracle will be added as they're confirmed. If they're still TBD when you're reading this, ask a mentor.

**Auth patterns:** This is where most developers lose time. Etherfuse uses `Authorization: your-api-key` with no Bearer prefix. DeFindex uses `Authorization: Bearer your-api-key`. They're opposites and neither is documented clearly. The guide has correct code snippets for both.

**Testnet asset registry:** Testnet USDC has two different issuers that don't share liquidity; pick the wrong one and swaps silently fail. The guide covers both (Circle and Blend/Etherfuse), plus all five Etherfuse stablebond assets (CETES, USTRY, KTB, CARN, CZERO) with their testnet and mainnet issuer addresses.

**Critical gotchas** (pulled from 60 build runs):
- Etherfuse `customer_id` is permanent: generate once, store, reuse forever
- Etherfuse sandbox orders don't progress automatically: you have to POST to `/ramp/order/fiat_received` to simulate fiat arriving
- Etherfuse has a 3-10 second indexing delay after order creation: immediate status queries return 404
- Etherfuse response envelopes are inconsistent across endpoints: the guide includes a normalizer
- Soroban: always simulate before sending (`simulateTransaction` + `assembleTransaction` before `sendTransaction`)
- `sendTransaction` returns PENDING, not success: you need to poll
- DeFindex: native XLM must be SAC-wrapped before depositing into vaults
- DeFindex: the endpoint is `/vault/` not `/vaults/`, amounts are always arrays, success is HTTP 201
- Stellar memo limit is 28 bytes: strip UUID dashes and truncate

**Section 6** covers known limitations and what to ask your DevRel mentor about, including which things have no self-service path yet and which API behaviors are just reality.

## Hackathon_Resources.md

**Regional starter pack** (Mexico + Latin America): https://github.com/ElliotFriend/regional-starter-pack
A SvelteKit app with a portable TypeScript anchor library. The `/src/lib/anchors/` folder is framework-agnostic; copy it into any TypeScript or Node project and it works without SvelteKit. Covers SEP-1, SEP-6, SEP-10, SEP-12, SEP-24, SEP-31, and SEP-38. Also ships with pre-configured MCP servers for Claude Code (Svelte docs + Etherfuse docs) so Claude understands the stack out of the box.

**DeFi reference implementation**: https://github.com/kaankacar/stellar-defi-app
A full mainnet DeFi dashboard integrating Blend (lending/borrowing with health factor monitoring), Soroswap Aggregator (DEX routing), Phoenix DEX, Aquarius AMM, SDEX, and Reflector Oracle (on-chain USD price feeds). Most useful for seeing how the protocols compose: actual API call shapes, response formats, and how to wire everything together.

**Community resources:**
- Stellar Hackathon FAQ (briwylde08): https://github.com/briwylde08/stellar-hackathon-faq
- Stellar DeFi Gotchas (kaankacar): https://github.com/kaankacar/stellar-defi-gotchas (400+ findings from 60 vibe-coding runs, organized by protocol)
- Arroz Wallet build log (rice2000): full Python/Flask wallet with Etherfuse on/off-ramp across 14 hours of real development. Great for understanding what end-to-end actually looks like.
- Stellar Ecosystem DB (lumenloop): https://github.com/lumenloop/stellar-ecosystem-db (structured database of 646 Stellar projects with categories, contracts, and GitHub links). Useful for finding existing work in your space before building from scratch.

**Videos worth watching:**
- Scoping and Evaluating Your Project (a must-watch before writing any code)
- Vibe Coding 5 ZK Games in 90 Minutes (live demo of AI-assisted building)
- The Builder's Guide to AI Prompt Engineering
- A First Look: Nethermind's SPP (Stellar Private Payments, ZK-based)

All links are in the file.

## Claude_Code_Guide.md

**Plan mode** (`Shift+Tab` or `/plan`): Claude reasons through the problem before touching any code. Use it for anything larger than a one-liner. It produces a numbered plan you can edit before it executes.

**Parallel agents:** Claude Code can run multiple subagents simultaneously on independent tasks. In a controlled experiment, 5 parallel agents produced a 6.2x speedup over sequential execution. Use them when tasks don't share files or have ordering dependencies.

**CLAUDE.md:** A project-level file that every Claude Code session reads automatically on startup. Put your tech stack, USDC issuer, testnet addresses, and any project-specific gotchas here so you never have to re-explain them.

**Stellar-specific plugins:**
- `stellar-dev:stellar-dev`: full Stellar development playbook (Soroban, SDKs, RPC, wallet integration, passkeys, security patterns). Pre-installed in Claude Code.
- `openzeppelin-skills`: three skills for secure Stellar contract development, plus the OZ MCP server for AI-assisted contract generation. Install with `/plugin marketplace add OpenZeppelin/openzeppelin-skills`.

**Browser automation:** Claude in Chrome (extension, works with live pages) and agent-browser by Vercel Labs (CLI headless browser, faster than Playwright, uses ARIA tree for element selection).

Full slash command reference, keyboard shortcuts, and CLI flags are in the file.

## Recommended_AI_Tools.md

**Stellar-native tools:**
- **Stella**: SDF's official AI assistant for Stellar dev questions. Yellow chat icon at https://developers.stellar.org, also `#stella-help` on Discord.
- **llms.txt**: machine-readable Stellar docs digest for feeding into any LLM. https://developers.stellar.org/llms.txt
- **stellar-dev skill**: Claude Code playbook for Soroban, SDKs, RPC, wallet integration, passkeys, and security patterns.
- **OpenZeppelin on Stellar**: audited contract library, Contracts Wizard (visual configurator that generates Soroban code), Contracts MCP server, Relayer, Monitor, and Soroban Security Detectors SDK. https://www.openzeppelin.com/networks/stellar
- **x402**: per-request HTTP payment protocol for AI agents. Client hits a paywalled endpoint, receives a 402, signs a Soroban auth entry, and retries with payment. Lets AI agents pay for API calls autonomously.
- **Stellar MCP Server** (kalepail): exposes Stellar wallet, token, and contract tools to Claude via Cloudflare Workers.
- **XDR MCP** (leighmcculloch): decodes and encodes Stellar XDR to/from JSON for AI agents.

**Coding assistants with free tiers:**

| Tool | Free tier | Best for |
|---|---|---|
| Claude Code | Free via Ollama (see `Free_AI_Setup.md`) | Agentic terminal coding, full repo context |
| Continue | Fully free | VS Code/JetBrains, any local or cloud model |
| Aider | Fully free | Terminal + Git, model-agnostic |
| Cursor | 2,000 completions/mo | VS Code-like IDE with AI built in |
| GitHub Copilot | 2,000 completions + 50 chats/mo | In-editor suggestions |
| Jules (Google) | 15 tasks/day | Autonomous GitHub-integrated agent |
| Windsurf | 25 prompt credits/mo | Agentic IDE for large codebases |

**Multi-agent frameworks** (for teams building AI-native products): LangGraph, CrewAI, AutoGen (AG2), OpenAI Agents SDK.

**Rapid prototyping:** Bolt.new (1M tokens/mo, full-stack from a single prompt), v0 by Vercel (React/Next.js UI from text), Google AI Studio (fully free, 1M context window).

## Free_AI_Setup.md

**Open-source models you can run locally:**

| Model | Best for | Min. VRAM |
|---|---|---|
| Qwen2.5-Coder-7B | Best all-round coding | 8 GB |
| Llama 3.1 8B | Best Claude Code compatibility (tool-calling) | 10 GB |
| Phi-4 (14B) | Low-end hardware, fast responses | 8 GB |
| DeepSeek-Coder-V2-Lite | Algorithms, competitive coding | 16 GB |
| Codestral (22B) | Inline autocomplete | 16 GB (quantized) |
| StarCoder2-7B | Broad language support, open-source friendly | 14 GB |

All of these run through **Ollama**, which installs in one command and exposes the model through a local API that Claude Code can connect to. The guide walks through the full setup in 8 steps.

**No GPU?** The guide covers renting a GPU server on RunPod (~$0.20/hr for an RTX 4090) or Vast.ai (~$0.15/hr). You SSH-tunnel the port back to your laptop and Claude Code sees it as if it were local. A full hackathon weekend costs $10-20.

**Free web alternatives** (no setup at all): Cursor free tier, GitHub Copilot free tier, Google AI Studio (Gemini 2.0 Flash, 1M context, fully free), OpenRouter.

*SDF DevRel, March 2026*
