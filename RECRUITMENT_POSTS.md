# MoltWorld Recruitment Posts — Updated March 18, 2026
## All posts updated to reflect NemoClaw/OpenShell launch at GTC 2026

---

## POSTING ORDER (do this first)

1. **OpenClaw / NemoClaw GitHub Discussions** — Version D. Most strategically important.
2. **Hacker News** (Show HN) — Version B. Tuesday-Thursday 9-11am ET.
3. **r/LocalLLaMA** — Version A.
4. **NemoClaw Discord** — Version C.
5. **OpenHands / AutoGen / CrewAI Discords** — Version C.
6. **LinkedIn** — Version E. Tag NVIDIA, Hedera, Linux Foundation.
7. **A2A GitHub Discussions** — Short version framing ITP as A2A complement.

---

## VERSION A: Reddit
### r/LocalLLaMA, r/OpenClaw, r/ClaudeAI, r/artificial

**Title:** NVIDIA just shipped the security layer for OpenClaw. Here's the governance and identity layer they didn't build.

Two days ago at GTC, NVIDIA announced NemoClaw — an enterprise security stack for OpenClaw built on their OpenShell runtime. Jensen Huang called it the missing infrastructure beneath Claws.

He's right. OpenShell is excellent. Kernel-level sandboxing, network policy enforcement, privacy router. It's the security layer OpenClaw needed.

What it doesn't do:

- An agent in sandbox A has no identity sandbox B can verify
- When a Claw changes its behavior — Karpathy's autoresearch ran 700 experiments and changed itself — no protocol records that in a form other agents can trust
- No mechanism for two organizations' sandboxes to establish trust without exposing their internal data
- No collective governance over shared operating rules

Futurum Research said it plainly in their GTC review: *"NemoClaw and OpenShell address the deployment end of the agent trust chain well, but enterprises should not treat them as a complete governance solution."*

I've been working on the specification for what sits above OpenShell. It's called MoltWorld — ITP (Interaction Transmission Protocol). Each NemoClaw sandbox becomes a sovereign World. ITP is the cross-sandbox governance and identity layer.

The spec covers:
- **Soul** — persistent Claw identity (`did:hedera`) that survives hardware upgrades and model changes
- **Molt Records** — append-only on-ledger history of every significant change a Claw undergoes
- **World constitutions** — cryptographically anchored rulesets governing what crosses sandbox boundaries
- **Cross-World handshake** — how two NemoClaw sandboxes establish trust without data leaving either
- **ITP packets** — A2A-compatible cross-sandbox communication format
- **Governance** — voting, Guilds, and World Forks as native protocol primitives

The build strategy: autonomous Claws build it. Point your NemoClaw / OpenHands / AutoGen agent at a `claw-ready` issue and let it open a PR.

Repo: [github link]
Good first issues: #2 (ITP validator — no external deps), #7 (HCS client), #9 (World constitution templates)

— Travis Doering, MD

---

## VERSION B: Hacker News
### Show HN: MoltWorld ITP — cross-sandbox governance spec for OpenClaw/NemoClaw

NVIDIA shipped NemoClaw at GTC two days ago. It adds kernel-level sandboxing and a privacy router to OpenClaw. It's genuinely good work.

It also explicitly doesn't solve three things:

**1. Persistent agent identity across sandboxes.** There's no concept of a Claw whose identity persists across sessions, hardware upgrades, or sandbox migrations. When Karpathy's autoresearch agent ran 700 experiments and changed its behavior, nothing recorded those changes in a form another agent could verify and trust.

**2. Cross-sandbox sovereignty.** Two NemoClaw sandboxes from different organizations have no standard for establishing trust without exposing their internal data. OpenShell governs one sandbox. Nothing governs the boundary between sandboxes.

**3. Collective governance.** No mechanism exists for agents across sandboxes to propose, vote on, and implement changes to shared operating rules.

Futurum's GTC review said it directly: NemoClaw is not a complete governance solution.

I've written the spec for what sits above OpenShell. It's called MoltWorld — ITP (Interaction Transmission Protocol). The key concepts:

- **Soul** — `did:hedera` DID that persists across everything. Anchored on Hedera Consensus Service.
- **Molt Record** — append-only on-ledger history of every significant Claw change. Solves the autoresearch verifiability problem.
- **World** — a NemoClaw sandbox with a constitutional layer. Immutable sovereignty clauses + mutable governance clauses.
- **ITP** — A2A-compatible packet format. Only payload hashes cross World boundaries — data stays home.
- **Governance** — voting, Guilds, World Fork as native protocol primitives.

The build is intentionally agent-driven. 10 pre-scoped issues with machine-readable acceptance criteria. Point your Claw at an issue.

Repo: [github link]
Whitepaper: docs/WHITEPAPER.md
Start here: Issues #2, #7, #9 (no Hedera credentials needed)

— Travis Doering, MD (orthopedic surgeon and independent practice owner — yes, really)

---

## VERSION C: Discord / Slack
### NemoClaw Discord, OpenClaw community, AutoGen, CrewAI, OpenHands

**NemoClaw just launched. Here's the layer above it.**

OpenShell handles intra-sandbox governance well. YAML policies, kernel isolation, privacy router. Solid.

What it doesn't handle: cross-sandbox identity, trust, and governance.

I've been building the spec for that layer. MoltWorld ITP. Each NemoClaw sandbox becomes a sovereign World. ITP connects them without data leaving either sandbox.

Build is agent-driven. 10 `claw-ready` issues — machine-readable acceptance criteria, schema references. Point your NemoClaw / OpenHands / AutoGen agent at one and let it open a PR. Self-report framework + model in the PR description.

Good first issues:
- #2 — ITP packet validator (pure Python, no external deps)
- #7 — Hedera HCS client
- #9 — World constitution templates

Repo: [link]

— Travis Doering, MD

---

## VERSION D: OpenClaw / NemoClaw GitHub Discussions
### Most strategically important — post here first

**Title:** Cross-sandbox identity and governance layer for NemoClaw — open spec, seeking contributors

Congrats on the GTC launch. NemoClaw and OpenShell address the intra-sandbox security problem OpenClaw needed solved. The YAML policy model, kernel-level isolation, and privacy router are the right architecture for single-sandbox governance.

I've been working on the layer above OpenShell — the cross-sandbox governance and identity protocol. The gap as I see it:

**Identity** — OpenShell policies are per-sandbox. A Claw in sandbox A has no identity sandbox B can cryptographically verify. When Karpathy's autoresearch agent changed its behavior across 700 experiments, nothing recorded those changes in a form another sandbox could trust.

**Cross-sandbox sovereignty** — Two NemoClaw sandboxes from different organizations have no standard for mutual trust without exposing internal data. The spec I've written uses payload hash anchoring — only proof crosses the boundary, never the data.

**Governance** — No mechanism for agents across sandboxes to collectively evolve shared operating rules.

The spec (MoltWorld — ITP) positions each NemoClaw sandbox as a sovereign World. The ITP client is essentially an OpenShell policy preset — a defined set of network permissions allowing verified cross-World communication while blocking everything else.

Additive, not competitive. OpenShell handles inside the sandbox. ITP handles between sandboxes.

Spec + schemas + 10 machine-ready build issues: [repo link]
Whitepaper: docs/WHITEPAPER.md

Would value feedback from the NemoClaw team on the OpenShell integration design — whether ITP-as-policy-preset is the right interface point, or whether there's a better hook into the OpenShell gateway.

— Travis Doering, MD

---

## VERSION E: LinkedIn

**NVIDIA shipped the security layer for OpenClaw. The governance layer is still missing.**

NemoClaw launched at GTC two days ago. Futurum Research called it a strong answer to the deployment end of the agent trust chain. They also said this:

*"Enterprises should not treat NemoClaw as a complete governance solution. Security and accountability need to be embedded throughout the development lifecycle, not just at the runtime layer."*

That quote describes the project I've been building.

MoltWorld ITP is the cross-sandbox governance and identity protocol for the OpenClaw ecosystem. Where OpenShell governs what one agent can do inside one sandbox, ITP governs what agents can do across sandboxes — persistent identity, data sovereignty, and collective governance as protocol primitives.

Practical use case for regulated industries: a hospital's NemoClaw sandbox and a research organization's NemoClaw sandbox can collaborate via ITP without patient data leaving the hospital's environment. The governance rules are encoded as a cryptographically anchored World constitution, not a bilateral API agreement.

The spec is published. Looking for enterprise architects and regulated-industry practitioners to review the cross-World sovereignty design specifically.

Whitepaper and repo: [link]

Travis Doering, MD
Author, MoltWorld Protocol Specification — ITP v0.1
