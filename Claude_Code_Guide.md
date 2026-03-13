# Claude Code Guide

A practical reference for using Claude Code effectively during the hackathon: key commands, plan mode, parallel agents, plugins, browser automation, and videos worth watching.


## 1. Core Workflow Concepts

### Plan Mode

Activate with `/plan` or `Shift+Tab`. Claude reasons through the problem before taking any action: no code written, no files touched, just thinking. Use it for any task larger than a one-liner: new features, refactors, multi-file changes.

In plan mode, Claude produces a numbered plan you can review and edit before confirming. Once you approve, it executes. This catches misunderstandings before they become wasted work.

### Parallel Agents

Claude Code can spawn multiple subagents that run simultaneously on independent tasks. In the DevRel experiment (kaankacar), 3 parallel agents cut wall-clock time by roughly 40% on a 145-minute total build.

**The pattern that works:**

```
[Plan mode: 20 min] → [Scaffold agent] → [3 parallel agents] → [Integration agent] → [Browser smoke test]
```

After your scaffold is in place, use this prompt to trigger the parallel phase:

```
The scaffold is in place. Divide the remaining work into three independent tracks:

Track 1: Core logic and tests
- [list the pure-logic modules]
- Write Vitest unit tests for each module as you go

Track 2: State management and routing
- [list the stores, state singletons, and routing logic]

Track 3: UI components
- [list the pages and components]
- Use the stores from Track 2; do not duplicate state

Spawn one agent per track and run them in parallel.
Once all three complete, spawn a fourth agent to wire everything together and run the full test suite.
```

Use parallel agents when:
- Tasks don't share files or have ordering dependencies
- You're doing research + implementation simultaneously
- You want to explore multiple approaches at once

**Why plan mode first matters:** The 20-minute upfront plan session is what makes parallelization possible. Without it, agents make conflicting assumptions about state shape and file structure. With it, each agent works from the same blueprint and the integration agent has almost nothing to fix.

### CLAUDE.md

A project-level instruction file that every Claude Code session reads automatically on startup. Put it at the repo root.

What to include:
- Tech stack and versions
- Project conventions (naming, folder structure, code style)
- Gotcha notes specific to your project (e.g., which USDC issuer you're using)
- Testnet addresses and API endpoint overrides
- Anything you'd otherwise have to re-explain every session

See `Starter_Prompts.md` for a ready-to-fill CLAUDE.md template for Stellar projects.


## 2. Full Command Reference

### Slash Commands

| Command | What it does |
|---|---|
| `/plan` | Enter plan mode: Claude reasons before acting |
| `/clear` | Clear conversation history, start fresh |
| `/compact` | Compress conversation to save context (keeps working memory, drops verbosity) |
| `/memory` | View and edit Claude's persistent memory |
| `/cost` | Show token usage and estimated cost for the session |
| `/plugin` | List installed plugins |
| `/plugin marketplace add owner/repo` | Install a plugin from GitHub |
| `/plugin install path/to/plugin` | Install a plugin from local path |
| `/model` | Switch the active model |
| `/help` | Show help |
| `/status` | Show current session status |
| `/vim` | Toggle vim keybindings |
| `/doctor` | Diagnose configuration issues |
| `/login` | Authenticate with Anthropic |
| `/logout` | Sign out |
| `/review` | Review code changes |
| `/pr` | Create a pull request |
| `/commit` | Create a git commit |
| `/test` | Run tests |
| `/bug` | Report a bug |
| `/explain` | Explain selected code |
| `/fix` | Fix a bug or error |
| `/todo` | Manage a task list |

### Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| `Shift+Tab` | Toggle plan mode |
| `Escape` | Interrupt current operation |
| `Ctrl+R` | Search command history |
| `Ctrl+C` | Cancel current operation |
| Up / Down arrows | Navigate command history |

### Useful CLI Flags

| Flag | What it does |
|---|---|
| `--print` | Non-interactive output mode (pipe-friendly) |
| `--model` | Specify model at startup |
| `--max-tokens` | Set max output tokens |
| `--allowedTools` | Restrict which tools Claude can use |
| `--dangerouslySkipPermissions` | Skip confirmation prompts (use carefully) |

### Pro Tips

- **CLAUDE.md is load-bearing.** The more project context you put there, the less you re-explain per session. Treat it like a living document.
- **`/compact` vs `/clear`:** Use `/compact` when you want Claude to keep working on the same task but the context is getting long. Use `/clear` when you're starting a completely new task.
- **Chain tasks:** End one task with a clear handoff note ("now do X next") and Claude will carry context into the next step without you re-explaining the codebase.


## 3. Plugins and Skills

### What's What

- **Plugin:** Bundles one or more skills together with optional MCP servers. Installed from GitHub via `/plugin marketplace add owner/repo`.
- **Skill:** A reusable prompt playbook for a specific task. Invoked by name or with `skill-name:action` syntax.
- **MCP server:** Gives Claude access to external tools and APIs (databases, web browsers, documentation, contract generation, etc.).

### Relevant Plugins for Stellar Hackathon

| Plugin / Skill | What it does | Install |
|---|---|---|
| `stellar-dev:stellar-dev` | Full Stellar development playbook covering 8 modules: Soroban contracts (Rust), RPC vs Horizon, frontend + wallet integration, classic assets + SAC, common pitfalls, security checklist, testing strategy, ecosystem catalog. | Pre-installed in Claude Code; invoke by name |
| `openzeppelin-skills` | Three skills: `setup-stellar-contracts`, `upgrade-stellar-contracts`, `develop-secure-contracts`. Installs OZ MCP server for AI-assisted contract generation. | `/plugin marketplace add OpenZeppelin/openzeppelin-skills` |

### How to Invoke a Skill

Type the skill name in the chat or use the colon syntax: `stellar-dev:stellar-dev`. Claude will load the playbook and apply it to your current task.


### What's in stellar-dev

The skill loads 8 reference modules on demand. Invoke the skill and tell Claude which area you're working in:

| Module | Invoke when |
|---|---|
| `contracts-soroban` | Writing or debugging a Soroban contract in Rust: storage types, TTL, auth, events, build/deploy, unit testing |
| `api-rpc-horizon` | Setting up API access: full method references for both servers, migration guide, historical data fallbacks |
| `frontend-stellar-sdk` | Connecting wallets (Freighter, Wallets Kit v2), building/signing/submitting transactions, passkey smart accounts |
| `stellar-assets` | Issuing a classic asset, SAC address derivation, using an asset inside a Soroban contract, SEP standards |
| `common-pitfalls` | Hit a confusing error or something works locally but not on testnet: 15 documented gotchas with fixes |
| `security` | Pre-deploy review: missing auth, reinitialization, overflow, storage collisions, TTL as attack vector |
| `testing` | Unit tests with soroban-sdk testutils, local Quickstart Docker, testnet setup, CI/CD template |
| `ecosystem` | Finding integration targets: DeFi protocols, oracles, wallets, indexing tools, contract libraries |


## 4. Browser Automation

Two options depending on your use case.

### Claude in Chrome (Extension)

A Chrome extension that gives Claude access to your active browser tab. Claude can read page content, click elements, fill forms, and scrape live data.

Good for: interacting with web UIs, testing deployed apps, pulling data from live pages, automating repetitive browser tasks.

Setup: install the extension from the Chrome Web Store, then Claude gets `mcp__claude-in-chrome__*` tools automatically.

### Integration Testing with Claude in Chrome

This is the most underused capability in a typical hackathon session. Claude can open your running app in a browser, click through every user flow, and verify on-chain results, without you touching the keyboard.

**Full smoke test prompt (paste after your dev server is running):**

```
Open http://localhost:5173 in Chrome.

Run this smoke test sequence and report pass/fail for each step:

1. Create wallet: click "Create New Wallet", note the generated mnemonic phrase
2. Friendbot: call Friendbot for the new wallet's public key, confirm XLM balance appears
3. Trustline: click "Add CETES trustline", confirm the transaction succeeds on-chain
4. Lock/Unlock: click "Lock Wallet", re-enter the password, confirm the app returns to the dashboard
5. Balance display: confirm XLM balance is visible in the wallet panel

For each step: describe what you see, what you did, and whether it passed.
If a step fails, paste the error message or console output.
```

**Known friction point:** Programmatic typing via the MCP `type` action fires keyboard events but not the HTML `input` event. If your framework listens to `oninput` (Svelte does), typed values won't trigger reactivity. Fix:

```javascript
// After typing into an input, dispatch a synthetic input event
const input = document.querySelector('input[name="password"]');
input.value = 'your-value';
input.dispatchEvent(new Event('input', { bubbles: true }));
```

Tell Claude: "After typing into any input field, dispatch a synthetic input event to trigger framework reactivity."

### agent-browser (vercel-labs)

CLI-based headless browser built for AI agents, written in Rust. Works with any browser, not Chrome-specific. Faster than Playwright and Puppeteer for AI-driven workflows. Uses the ARIA accessibility tree for element selection instead of CSS selectors.

Good for: server-side automation, CI pipelines, headless scraping, any context where you can't use a visible browser.

- Repo: https://github.com/vercel-labs/agent-browser
- Install: `npm install -g @vercel/agent-browser` or via Homebrew
