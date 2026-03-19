# MoltWorld — New Session Bootstrap
## Read this first. Then paste the PROMPT below into a new Claude chat with GitHub connector enabled.

---

## What This Is

You are picking up a project mid-stream. MoltWorld is an open protocol specification for sovereign, interoperable AI agent environments. The communication layer is called ITP (Interaction Transmission Protocol). It was authored by Travis Doering, MD.

All the documents you need are in this zip. Your job in this session is to create the GitHub repository and populate it completely using the GitHub MCP connector.

---

## Before You Start

Confirm the GitHub connector is enabled in your Claude session (Settings → Connectors → GitHub). If it isn't, enable it before pasting the prompt.

You will need:
- Your GitHub username
- The GitHub repository name: `moltworld`

---

## PASTE THIS PROMPT INTO THE NEW CLAUDE SESSION

---

I need you to create a GitHub repository called `moltworld` under my account and fully populate it. This is for an open protocol specification called MoltWorld — ITP (Interaction Transmission Protocol), authored by Travis Doering, MD.

Use the GitHub connector to do all of this. Do not ask me to run CLI commands.

Here is exactly what to build:

**Step 1 — Create the repository**
- Name: `moltworld`
- Description: "Sovereign, interoperable environment layer for autonomous AI agents. ITP v0.1 spec + reference implementation."
- Visibility: Public
- License: Apache 2.0

**Step 2 — Create these labels**
- `claw-ready` (green #0E8A16) — "Scoped and ready for autonomous Claw execution"
- `good-first-issue` (purple #7057FF) — "Recommended starting point for a new Claw"
- `needs-human` (red #D93F0B) — "Requires human architectural judgment"
- `blocked` (yellow #E4E669) — "Waiting on another issue"
- `spec-question` (blue #0075CA) — "Something in the whitepaper needs clarification"

**Step 3 — Upload these files** (contents are in the attached documents)
- `README.md` → root (use contents of `repo_files/README.md`)
- `CONTRIBUTING.md` → root (use contents of `repo_files/CONTRIBUTING.md`)
- `LICENSE` → root (Apache 2.0, copyright Travis Doering MD 2026)
- `docs/WHITEPAPER.md` → use contents of `repo_files/WHITEPAPER.md`
- `schema/itp_packet.json` → use contents of `repo_files/schema/itp_packet.json`
- `schema/soul_manifest.json` → use contents of `repo_files/schema/soul_manifest.json`
- `schema/molt_record.json` → use contents of `repo_files/schema/molt_record.json`
- `schema/world_constitution.json` → use contents of `repo_files/schema/world_constitution.json`
- Create empty folders with `.gitkeep`: `src/moltworld/soul/`, `src/moltworld/itp/`, `src/moltworld/world/`, `src/moltworld/governance/`, `src/moltworld/molt/`, `src/moltworld/hedera/`, `src/moltworld/tests/`

**Step 4 — Create all 10 GitHub Issues** (full text for each is in `repo_files/GITHUB_ISSUES.md`)

Issue titles and labels:
1. "Soul Registration on Hedera" → `claw-ready`
2. "ITP Packet Schema Validation" → `claw-ready`, `good-first-issue`
3. "ITP Packet Builder" → `claw-ready`
4. "World Ruleset Engine" → `claw-ready`
5. "Cross-World Handshake" → `claw-ready`
6. "Molt Event Broadcaster" → `claw-ready`
7. "Hedera HCS Client" → `claw-ready`, `good-first-issue`
8. "A2A Compatibility Layer" → `claw-ready`
9. "World Constitution Template Generator" → `claw-ready`, `good-first-issue`
10. "Local Development Environment" → `claw-ready`

**Step 5 — Pin the repo and confirm**
When done, give me the live URL of the repo and confirm all 10 issues are visible.

All file contents are in the attached documents in this session. Work through the steps in order. If the GitHub connector requires my username, ask me before proceeding.

---

## Files In This Kit

| File | Purpose |
|---|---|
| `START_HERE.md` | This file — read first |
| `repo_files/README.md` | Repo front page |
| `repo_files/CONTRIBUTING.md` | Agent contribution guide |
| `repo_files/WHITEPAPER.md` | Full protocol specification (markdown version) |
| `repo_files/schema/itp_packet.json` | ITP packet JSON schema |
| `repo_files/schema/soul_manifest.json` | Soul identity schema |
| `repo_files/schema/molt_record.json` | Molt Event schema |
| `repo_files/schema/world_constitution.json` | World constitution schema |
| `repo_files/GITHUB_ISSUES.md` | All 10 issues ready to create |
| `RECRUITMENT_POSTS.md` | Posts for Reddit, HN, Discord, LinkedIn |
| `ACTION_PLAN.md` | Step-by-step launch sequence |
| `NEMOCLAW_NOTES.md` | Notes on NemoClaw reference implementation |
