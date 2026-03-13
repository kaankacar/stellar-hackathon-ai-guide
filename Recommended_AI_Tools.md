# Recommended AI Tools for the SDF Hackathon

A curated map of the AI tool ecosystem available during the hackathon. Covers Stellar-native integrations, coding assistants, and multi-agent frameworks. For local/open-source model setup, see `AI_Coding_Guide.md`.


## 1. Stellar-Native AI Tools

Start here. These are unique to the Stellar ecosystem.

| Resource | What it is | Where to find it |
|---|---|---|
| **Stella** | Official AI bot for Stellar dev questions (beta) | https://developers.stellar.org/docs/tools/developer-tools/ai-bot (yellow chat icon on docs site); also `#stella-help` on Discord |
| **llms.txt** | Machine-readable Stellar docs digest designed for feeding into LLMs | https://developers.stellar.org/llms.txt (covers Build, Learn, Tokens, Data, Tools, Networks, Validators) |
| **stellar-dev skill** | Claude Code skill (Jan 2026 playbook) covering Soroban, SDKs, RPC, wallet integration, passkeys, and security patterns | Invoke with `stellar-dev:stellar-dev` in Claude Code; repo: https://github.com/stellar/stellar-dev-skill |
| **OpenZeppelin skills** | Claude Code skills for secure Stellar contract development: `setup-stellar-contracts`, `upgrade-stellar-contracts`, and `develop-secure-contracts`. Installs optional MCP servers for AI-assisted contract generation. | Install: `/plugin marketplace add OpenZeppelin/openzeppelin-skills` in Claude Code; repo: https://github.com/OpenZeppelin/openzeppelin-skills |
| **Smart Account Kit** (kalepail) | TypeScript SDK for building passkey smart wallets on Soroban: `createWallet`, `connectWallet` (silent session restore), `signAndSubmit`, gasless tx via OZ Relayer, multiple signer types (passkeys, Ed25519, policies) | https://github.com/kalepail/smart-account-kit |
| **Stellar MCP Server** (kalepail) | MCP server running on Cloudflare Workers; exposes Stellar wallet, token, and contract tools to Claude and other AI clients | https://github.com/kalepail/stellar-mcp-server |
| **XDR MCP** (leighmcculloch) | MCP server that decodes and encodes Stellar XDR to/from JSON for AI agents | https://github.com/leighmcculloch/mcp-stellar-xdr |
| **x402** | Per-request HTTP payment protocol for AI agents, powered by Soroban auth entry signing | https://developers.stellar.org/docs/build/apps/x402 |

### x402: HTTP Payments for AI Agents

x402 repurposes the HTTP 402 Payment Required status into a real payment mechanism. AI agents can autonomously pay for API calls without human intervention.

- **How it works:** Client hits a paywalled endpoint, receives a 402 with payment instructions, signs a Soroban auth entry, and retries with the payment header.
- **Supported wallets:** Freighter, Albedo, Hana, HOT, Klever, OneKey
- **Facilitator:** OpenZeppelin Relayer plugin (testnet + mainnet); see OpenZeppelin on Stellar below
- **Live demo:** https://x402-stellar-491bf9f7e30b.herokuapp.com/
- **Official monorepo:** https://github.com/stellar/x402-stellar (facilitator service, simple-paywall demo in Express + React, Heroku deployment config, high-throughput channel account setup)
- **Community demo** (jamesbachini): https://github.com/jamesbachini/x402-Stellar-Demo (minimal local demo showing payer client + protected Express server + local facilitator; good for understanding the flow end-to-end)

### OpenZeppelin on Stellar

OpenZeppelin's full platform for building and securing Stellar smart contracts: https://www.openzeppelin.com/networks/stellar

- **Stellar Contracts Library:** Audited, reusable Soroban components for asset issuance, stablecoins, and DeFi (the Stellar equivalent of OZ's battle-tested Ethereum libraries)
- **Contracts Wizard:** Visual configurator that generates audited Soroban contract code instantly, no boilerplate
- **Contracts MCP:** MCP server at https://mcp.openzeppelin.com/ (gives AI agents direct access to OZ contract generation tools; auto-installed with the Claude Code plugin above)
- **Relayer:** Simplifies backend transaction flow for anchors and apps (this is the facilitator used by x402 above)
- **Monitor:** Real-time anomaly detection and automated responses for deployed contracts
- **Soroban Security Detectors SDK:** Static analysis with prebuilt vulnerability checks for Soroban contracts


## 2. AI Coding Assistants

Tools for using AI while writing code. Most have generous free tiers.

| Tool | Free Tier | Best For | Link |
|---|---|---|---|
| **Claude Code** | Paid (or free via Ollama; see `Free_AI_Setup.md`) | Terminal-based agentic coding, full repo context | https://claude.com/product/claude-code |
| **Continue** | Fully free (open source) | VS Code/JetBrains, works with any local or cloud model | https://continue.dev |
| **Cursor** | 2,000 completions/mo | VS Code-like IDE with AI built in | https://cursor.com |
| **Aider** | Fully free | Terminal + Git integration, model-agnostic | https://aider.chat |
| **Windsurf** | 25 prompt credits/mo | Agentic IDE for large codebases | https://windsurf.com |
| **Jules** (Google) | 15 tasks/day | Autonomous GitHub-integrated coding agent (Gemini 2.5 Pro) | https://jules.google.com |
| **GitHub Copilot** | 2,000 completions + 50 chats/mo | In-editor suggestions, integrates into VS Code | https://github.com/features/copilot |


## 3. Multi-Agent Orchestration Frameworks

For teams building AI-powered applications, not just using AI to write code, but shipping products where AI agents do the work.

| Framework | Style | Link |
|---|---|---|
| **LangGraph** | Graph-based stateful workflows with explicit control flow | https://github.com/langchain-ai/langgraph |
| **CrewAI** | Role-driven agent teams, beginner-friendly API | https://github.com/joaomdmoura/crewai |
| **AutoGen (AG2)** | Conversation-based multi-agent collaboration | https://github.com/ag2-ai/ag2 |
| **OpenAI Agents SDK** | Production-grade, built-in tracing and agent handoffs | https://openai.github.io/openai-agents-python/ |


## 4. Rapid Prototyping and App Builders

Go from idea to deployed app fast. Useful for MVPs and demos.

| Tool | Free Tier | Best For | Link |
|---|---|---|---|
| **Bolt.new** | 1M tokens/mo | Full-stack app generated from a single prompt | https://bolt.new |
| **v0** (Vercel) | $5 credits/mo | React/Next.js UI components from text descriptions | https://v0.app |
| **Google AI Studio** | Fully free | Paste large codebases into a 1M-context window, Gemini 2.0 Flash | https://aistudio.google.com |
