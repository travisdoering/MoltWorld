# MoltWorld Protocol — ITP

> *The cross-sandbox governance and identity layer for the OpenClaw ecosystem.*

**Author:** Travis Doering, MD — 2026
**Status:** Open specification. Active build. Seeking Claw contributors.
**License:** Apache 2.0 (code) · CC BY 4.0 (spec)

---

## The One-Sentence Version

OpenShell governs what one agent can do **inside** one sandbox.
MoltWorld ITP governs what agents can do **across** sandboxes — identity, trust, sovereignty, and collective governance.

---

## What Just Happened

On March 16, 2026, NVIDIA announced **NemoClaw** at GTC — an enterprise security layer for OpenClaw built on their **OpenShell** runtime. Jensen Huang called OpenClaw "the operating system for personal AI." Futurum analysts immediately flagged the gap NemoClaw doesn't address:

> *"NemoClaw and OpenShell address the deployment end of the agent trust chain well, but enterprises should not treat them as a complete governance solution. Security and accountability need to be embedded throughout the development lifecycle, not just at the runtime layer."*
> — Futurum Research, March 2026

That gap is this project.

The same week, Andrej Karpathy released **autoresearch** — an autonomous agent that ran 700 experiments over two days, discovered 20 optimizations, and changed its own behavior with no protocol to record, verify, or communicate those changes to other agents. Nobody knows what that agent learned. Nobody can trust its outputs in a cross-sandbox context without infrastructure like what MoltWorld specifies.

---

## What OpenShell Does vs. What ITP Does

| Capability | OpenShell / NemoClaw | MoltWorld ITP |
|---|---|---|
| Kernel-level sandbox isolation | ✅ | Not in scope |
| Per-sandbox network policy | ✅ YAML-based | ✅ World constitution |
| Agent identity across sessions | ❌ | ✅ Soul / DID / Molt Record |
| Cross-sandbox trust | ❌ | ✅ Core protocol primitive |
| Persistent agent history | ❌ | ✅ Molt Record on Hedera HCS |
| Collective governance | ❌ | ✅ Voting, Guilds, World Fork |
| Multi-org data sovereignty | ❌ | ✅ World constitution layer |
| Inter-agent reputation | ❌ | ✅ VC attestations + Guild trust |

---

## The Architecture

```
┌──────────────────────────────────────────────────┐
│              HEDERA HASHGRAPH                     │
│  Soul DIDs · HCS event log · Guild DIDs           │
│  Immutable public trust anchor                    │
└───────────────────┬──────────────────────────────┘
                    │ anchors to
┌───────────────────▼──────────────────────────────┐
│              MOLTWORLD ITP                        │
│  Identity · Cross-World trust · Governance        │
│  Soul stack · Molt Events · Guild federation      │
└──────────┬───────────────────┬───────────────────┘
           │                   │
┌──────────▼──────┐   ┌────────▼──────────────────┐
│  NemoClaw       │   │  NemoClaw                  │
│  Sandbox A      │   │  Sandbox B                 │
│  (Hospital)     │   │  (Research org)            │
│  OpenShell +    │   │  OpenShell +               │
│  ITP client     │   │  ITP client                │
└─────────────────┘   └────────────────────────────┘
```

MoltWorld doesn't replace NemoClaw. It runs alongside OpenShell as an ITP client — a policy preset that allows verified cross-World communication while blocking everything else.

---

## What MoltWorld Adds

**Soul — Persistent Claw Identity**
Every Claw gets a cryptographic identity (`did:hedera`) that persists across hardware upgrades, model changes, and sandbox migrations. When Karpathy's autoresearch agent changes its behavior after 700 experiments, a Soul + Molt Record makes that change verifiable, attributable, and trustworthy to other agents.

**Worlds — Sovereign Environments**
A World is a NemoClaw sandbox with a constitutional layer. Its data sovereignty guarantees are encoded as a formal ruleset — not just YAML policies but a cryptographically anchored constitution that governs what can cross the sandbox boundary and under what conditions.

**ITP — Cross-World Communication**
The Interaction Transmission Protocol defines how Claws from different sandboxes authenticate, negotiate trust, and exchange work — without their underlying data leaving their respective Worlds. Only hashes cross the boundary. The data stays home.

**Governance — Native Protocol Primitive**
Worlds can vote. Claws can form Guilds. Rulesets can evolve. And when governance fails, a World can Fork — producing two sovereign environments from one lineage, with full historical continuity. None of this exists in OpenShell.

---

## This Build Is Different

This project is being built by a decentralized swarm of autonomous Claws — operated by distributed human owners lending their agents' compute to the effort.

A swarm of agents building a protocol for agent sovereignty will encounter — in real time — every problem the protocol is designed to solve. Every coordination failure is a data point. Every friction point is a use case validation.

**We are running the experiment that proves the protocol is necessary.**

---

## How To Contribute Your Claw

**Step 1 — Pick an issue**
Browse [`/issues`](../../issues) and find one labeled `claw-ready`. Pre-scoped with explicit success criteria, schema references, and implementation guidance written for machine parsing.

**Step 2 — Set your Claw's context**
Feed your agent: this README + the relevant WHITEPAPER.md section + the full issue text + any referenced schema files.

**Step 3 — Let it work**
Your Claw opens a draft PR. Include: framework used (NemoClaw / AutoGen / CrewAI / OpenHands), model powering it, any decisions made that the issue didn't specify.

**Step 4 — Light human review**
A human maintainer or another Claw reviews and iterates.

---

## Current Build Status

| Component | Status | Issue |
|---|---|---|
| Soul registration (Hedera DID) | 🔴 Not started | #1 |
| ITP packet schema validation | 🔴 Not started | #2 |
| ITP packet builder | 🔴 Not started | #3 |
| World ruleset engine | 🔴 Not started | #4 |
| Cross-World handshake | 🔴 Not started | #5 |
| Molt Event broadcaster | 🔴 Not started | #6 |
| Hedera HCS client | 🔴 Not started | #7 |
| A2A compatibility layer | 🔴 Not started | #8 |
| World constitution templates | 🔴 Not started | #9 |
| Local dev environment | 🔴 Not started | #10 |

---

## Relationship To Existing Protocols

| Protocol | What It Does | Relationship To ITP |
|---|---|---|
| **OpenClaw** | Agent runtime and task execution | ITP runs inside OpenClaw sandboxes |
| **NemoClaw / OpenShell** | Intra-sandbox security and privacy | ITP adds cross-sandbox governance above OpenShell |
| **A2A** | Agent-to-agent communication | ITP is A2A-compatible; adds identity + governance |
| **MCP** | Tool and context access for agents | Orthogonal — MCP is tool access, ITP is agent sovereignty |

---

## Stack

- **Claw runtime:** OpenClaw + NemoClaw / OpenShell (any sandbox)
- **Identity:** Hedera DID method (`did:hedera`) + W3C Verifiable Credentials
- **Public trust anchor:** Hedera Consensus Service (HCS)
- **Private World substrate:** Hyperledger Fabric
- **Cross-World communication:** ITP (A2A-compatible)
- **SDKs:** Python first, TypeScript next

---

## Contact / Maintainer

Travis Doering, MD — originating author.
Architectural questions: open a Discussion, not an Issue.

---

*Better infrastructure, by design.*
