# Stellar Hackathon AI Guide

This repo exists because access to good AI tooling shouldn't depend on having a paid subscription or a high-end laptop. Whether you're coming in with a Claude Pro account or a mid-range laptop and no budget, you should be able to build something real at this hackathon.

These guides cover everything from free local model setup to Stellar-specific protocol gotchas. Read what's relevant to you, skip what isn't.

---

## Where to Start

**Don't have a paid AI subscription?** Start with `Free_AI_Setup.md`. It walks through running capable open-source models locally (or on a cheap GPU server) and connecting them to your coding tools, at no cost.

**About to write code?** Read `Dev_Setup_Guide.md` first. It has the API keys, testnet contract addresses, auth patterns, and common gotchas that have blocked developers for hours in the past. Five minutes here saves a lot of pain later.

**Want to know what tools exist?** `Recommended_AI_Tools.md` maps the full AI ecosystem available to you: Stellar-native integrations, coding assistants, multi-agent frameworks, and rapid prototyping tools.

---

## The Files

| File | What it covers |
|---|---|
| `Dev_Setup_Guide.md` | API keys, testnet addresses, auth patterns, setup gotchas, known limitations |
| `Hackathon_Resources.md` | Ecosystem links, regional starter pack (MXN rails), reference implementations, videos |
| `Claude_Code_Guide.md` | How to use Claude Code effectively: plan mode, parallel agents, commands, plugins |
| `Recommended_AI_Tools.md` | Stellar-native AI tools, coding assistants, multi-agent frameworks, app builders |
| `Free_AI_Setup.md` | Running AI locally for free: open-source models via Ollama, VPS setup, free web alternatives |

---

## Suggested Reading Order

If you're starting fresh:

1. `Free_AI_Setup.md` (if you need a free AI setup)
2. `Dev_Setup_Guide.md` (before writing any code)
3. `Hackathon_Resources.md` (to orient yourself in the Stellar ecosystem)
4. `Claude_Code_Guide.md` (if you're using Claude Code)
5. `Recommended_AI_Tools.md` (to explore what else is available)

---

## Quick Picks

**Starting with Mexican peso rails:** The regional starter pack in `Hackathon_Resources.md` is the fastest path. It has Etherfuse, AlfredPay, and BlindPay already wired up.

**Building DeFi on Stellar:** The reference implementation in `Hackathon_Resources.md` integrates Blend, Soroswap, Phoenix, Aquarius, SDEX, and Reflector Oracle in one working app.

**Using Claude Code for the first time:** The three things that matter most are plan mode (`Shift+Tab`), CLAUDE.md, and parallel agents. All in `Claude_Code_Guide.md`.

**Getting blocked on a protocol:** Check Section 5 and Section 6 of `Dev_Setup_Guide.md` before spending more than 10 minutes debugging. Most blockers are documented there. If not, find a mentor.

---

*Built for the SDF Hackathon, Mexico, March 2026.*
