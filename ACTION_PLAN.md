# MoltWorld Launch — Step By Step Action Plan
## Travis Doering, MD

Everything below is in order. Do not skip steps.

---

## PHASE 1 — Repository (2-3 hours)

### Step 1: Create GitHub Account / Organization
- Go to github.com
- Create account if needed: username suggestion `travisdo` or `moltworld-protocol`
- Create a new **public** repository named `moltworld`
- Description: "Sovereign, interoperable environment layer for autonomous AI agents. ITP v0.1 spec + reference implementation."
- License: Apache 2.0
- Initialize with README: NO (you're uploading your own)

### Step 2: Upload The Documents
Upload everything in this folder to the repo root:
- `README.md` → repo root
- `CONTRIBUTING.md` → repo root
- `docs/WHITEPAPER.md` → create `/docs/` folder, upload there
  *(Copy your whitepaper content into a WHITEPAPER.md file)*
- `schema/itp_packet.json` → create `/schema/` folder
- `schema/soul_manifest.json` → `/schema/`
- `schema/molt_record.json` → `/schema/`
- `schema/world_constitution.json` → `/schema/`

### Step 3: Create Empty Folder Structure
Create these empty folders with a `.gitkeep` file in each:
- `/src/moltworld/soul/`
- `/src/moltworld/itp/`
- `/src/moltworld/world/`
- `/src/moltworld/governance/`
- `/src/moltworld/molt/`
- `/src/moltworld/hedera/`
- `/src/moltworld/tests/`

### Step 4: Create The GitHub Issues
Go to your repo → Issues → New Issue
Create one issue for each entry in `issues/GITHUB_ISSUES.md`
Copy the title, body, and labels exactly.

Labels to create first (Issues → Labels → New Label):
- `claw-ready` (green)
- `good-first-issue` (purple)
- `needs-human` (red)
- `blocked` (yellow)
- `spec-question` (blue)

### Step 5: Pin The Repo
Go to your GitHub profile → Customize your profile → Pin `moltworld`

---

## PHASE 2 — Test Your Own Claw First (1-2 hours)

Before recruiting anyone else, run your own autonomous agent against one issue.
This validates that the issues are actually agent-executable and finds obvious gaps.

### Step 6: Pick An Agent Framework
Easiest options to get running quickly:
- **OpenHands** (formerly OpenDevin): https://github.com/All-Hands-AI/OpenHands
  - Runs locally via Docker
  - Can read GitHub issues and open PRs
  - Good documentation

- **AutoGen**: https://github.com/microsoft/autogen
  - Python library
  - More setup required but more flexible

Recommendation: Start with OpenHands. It has the most turnkey GitHub integration.

### Step 7: Point Your Claw At Issue #2
Issue #2 (ITP packet validator) has no external dependencies.
It's the best first test because it's self-contained.

Feed your agent:
1. The issue text
2. `README.md`
3. `WHITEPAPER.md` Section 4.1
4. `schema/itp_packet.json`

Tell it to implement `src/moltworld/itp/validator.py` and open a draft PR.

### Step 8: Review What It Produces
Note:
- What did the agent get right?
- What did the agent get wrong?
- What was ambiguous in the issue that the agent had to guess?
- How long did it take?

Update the issue text to fix any ambiguities you found.

---

## PHASE 3 — Register For Hedera Testnet (30 minutes)

You'll need this to test Issues #1, #5, #6, #7 against real infrastructure.
It's free.

### Step 9: Get Hedera Testnet Credentials
- Go to: https://portal.hedera.com
- Create account
- Get testnet HBAR (free)
- Note your Operator ID and Operator Key
- Add to your local `.env` file (never commit this to GitHub)

---

## PHASE 4 — Recruit (post after Phase 1 and 2 are done)

### Step 10: Post To Communities
Post the relevant version from `RECRUITMENT_POSTS.md` to each platform.
Do NOT post until the GitHub repo is fully set up with all issues live.

**Posting order (most likely to get technical early adopters):**

1. **Hacker News** (Show HN) — post Tuesday-Thursday 9am-11am US Eastern
   Best timing for technical audience. One shot at the front page.

2. **r/LocalLLaMA** — most technically sophisticated AI community on Reddit
   They run local models and autonomous agents. Exact right audience.

3. **r/ClaudeAI** — relevant given the Anthropic connection

4. **AutoGen Discord** — https://discord.gg/autogen
   Direct developer community. Use Version C post.

5. **OpenHands Discord** — direct developer community

6. **CrewAI Discord** — direct developer community

7. **LinkedIn** — use Version D. Tag Hedera and Hyperledger official accounts.

8. **A2A GitHub Discussions** — post directly in the A2A repo discussions
   Frame as: "Governance and sovereignty layer complement to A2A — feedback welcome"
   This is the most important post strategically.

### Step 11: Respond To Every Comment In The First 48 Hours
Every question gets a response. Every critique gets engaged.
This is the difference between a thread that dies and one that grows.

---

## PHASE 5 — Ongoing

### Step 12: Merge PRs As They Come In
You are the human maintainer. Your job is:
- Review PRs for architectural alignment with the whitepaper
- Merge what's correct
- Leave specific feedback on what needs changing
- Update the build status table in README.md after each merge

### Step 13: Document What's Happening
You have a podcast about independent practice. You now also have a parallel story:
- Screenshot the first PR from an autonomous agent
- Note the framework and model
- Document what it got right and wrong
- This is your content

### Step 14: Post A Build Update After The First Month
Whatever has been built — working or not — post an honest update.
"Here's what a decentralized agent swarm produced in 30 days. Here's what it got right. Here's what it got wrong. Here's what this tells us about the current state of autonomous agent systems."

That post will get more attention than the original announcement.

---

## WHAT THIS COSTS YOU

| Item | Cost |
|---|---|
| GitHub repo | Free |
| Hedera testnet | Free |
| OpenHands local run | Free (uses your API key — ~$1-5 of Claude/GPT tokens per test) |
| Your time (Phases 1-4) | ~6-8 hours |
| Ongoing maintenance | ~2-4 hours/week |

**Total out of pocket: effectively zero.**

---

## WHAT YOU GET

| Outcome | Value |
|---|---|
| Originator attribution on a live open protocol spec | Permanent |
| First dataset of autonomous agent swarm build behavior | Publishable |
| Community of people with skin in the outcome | Compounding |
| Validation or falsification of the architectural thesis | Directional |
| Podcast content that writes itself | Immediate |
| Position in the A2A ecosystem as the governance complement | Strategic |

---

## IF NOBODY SHOWS UP

Post a follow-up to Hacker News after 2 weeks:
"I tried to recruit autonomous agents to build an open protocol. Here's what happened."

Honest post-mortems on HN consistently outperform the original announcement.
You still win the content play either way.

---

*This document was produced in collaboration with Claude (Anthropic).*
*The ideas are Travis Doering's. The documents are ready to ship.*
