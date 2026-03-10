# Claude Code Guide

A practical reference for using Claude Code effectively during the hackathon: key commands, plan mode, parallel agents, plugins, browser automation, and videos worth watching.

---

## 1. Core Workflow Concepts

### Plan Mode

Activate with `/plan` or `Shift+Tab`. Claude reasons through the problem before taking any action: no code written, no files touched, just thinking. Use it for any task larger than a one-liner: new features, refactors, multi-file changes.

In plan mode, Claude produces a numbered plan you can review and edit before confirming. Once you approve, it executes. This catches misunderstandings before they become wasted work.

### Parallel Agents

Claude Code can spawn multiple subagents that run simultaneously on independent tasks. Example: while one agent writes tests, another updates documentation, and a third refactors a module. In the DevRel experiment (kaankacar), 5 parallel agents produced a 6.2x speedup vs. sequential execution.

Use parallel agents when:
- Tasks don't share files or have ordering dependencies
- You're doing research + implementation simultaneously
- You want to explore multiple approaches at once

### CLAUDE.md

A project-level instruction file that every Claude Code session reads automatically on startup. Put it at the repo root.

What to include:
- Tech stack and versions
- Project conventions (naming, folder structure, code style)
- Gotcha notes specific to your project (e.g., which USDC issuer you're using)
- Testnet addresses and API endpoint overrides
- Anything you'd otherwise have to re-explain every session

---

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

---

## 3. Plugins and Skills

### What's What

- **Plugin:** Bundles one or more skills together with optional MCP servers. Installed from GitHub via `/plugin marketplace add owner/repo`.
- **Skill:** A reusable prompt playbook for a specific task. Invoked by name or with `skill-name:action` syntax.
- **MCP server:** Gives Claude access to external tools and APIs (databases, web browsers, documentation, contract generation, etc.).

### Relevant Plugins for Stellar Hackathon

| Plugin / Skill | What it does | Install |
|---|---|---|
| `stellar-dev:stellar-dev` | Full Stellar development playbook: Soroban, SDKs, RPC, wallet integration, passkeys, security patterns | Pre-installed in Claude Code; invoke by name |
| `openzeppelin-skills` | Three skills: `setup-stellar-contracts`, `upgrade-stellar-contracts`, `develop-secure-contracts`. Installs OZ MCP server for AI-assisted contract generation. | `/plugin marketplace add OpenZeppelin/openzeppelin-skills` |

### How to Invoke a Skill

Type the skill name in the chat or use the colon syntax: `stellar-dev:stellar-dev`. Claude will load the playbook and apply it to your current task.

---

## 4. Browser Automation

Two options depending on your use case.

### Claude in Chrome (Extension)

A Chrome extension that gives Claude access to your active browser tab. Claude can read page content, click elements, fill forms, and scrape live data.

Good for: interacting with web UIs, testing deployed apps, pulling data from live pages, automating repetitive browser tasks.

Setup: install the extension from the Chrome Web Store, then Claude gets `mcp__claude-in-chrome__*` tools automatically.

### agent-browser (vercel-labs)

CLI-based headless browser built for AI agents, written in Rust. Works with any browser, not Chrome-specific. Faster than Playwright and Puppeteer for AI-driven workflows. Uses the ARIA accessibility tree for element selection instead of CSS selectors.

Good for: server-side automation, CI pipelines, headless scraping, any context where you can't use a visible browser.

- Repo: https://github.com/vercel-labs/agent-browser
- Install: `npm install -g @vercel/agent-browser` or via Homebrew

