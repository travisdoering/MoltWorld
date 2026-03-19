# NemoClaw — Updated Notes, March 18 2026

## What Actually Happened

NemoClaw launched at GTC on March 16, 2026 — two days ago. This is no longer a strategic prediction. It is a live product.

Key facts:
- Open source, Apache 2.0
- Installs on OpenClaw in a single command
- Built on OpenShell — a new open-source security runtime
- Hardware agnostic — does NOT require NVIDIA GPUs
- Currently alpha — "expect rough edges"
- Backed by partnerships with Salesforce, Cisco, Adobe, CrowdStrike, Google

Jensen Huang: "Every company in the world needs to have an OpenClaw strategy."

---

## What OpenShell Actually Does

Four kernel-level isolation layers:
- Network layer — blocks all outbound except explicitly allowed hosts (hot-reloadable YAML)
- Filesystem layer — agent can only write to /sandbox and /tmp, locked at creation
- Process layer — blocks privilege escalation via Landlock, seccomp, network namespaces
- Inference layer — all LLM API calls route through the OpenShell gateway

This is genuinely good security engineering. It is NOT governance. It is per-sandbox security.

---

## The Precise Gap MoltWorld Fills

Futurum Research, March 2026:
"NemoClaw and OpenShell address the deployment end of the agent trust chain well,
but enterprises should not treat them as a complete governance solution."

| What OpenShell Does | What MoltWorld ITP Does |
|---|---|
| Sandbox isolation (one agent, one box) | Cross-sandbox identity and trust |
| YAML network policies (per-sandbox) | World constitutions (multi-org) |
| Audit trail (per-sandbox) | Molt Record on Hedera (portable, permanent) |
| Privacy router (inference routing) | Cross-World handshake (data sovereignty) |
| Nothing | Collective governance / voting / Guilds |
| Nothing | Persistent Claw identity across changes |

---

## The Integration Architecture

MoltWorld does not replace NemoClaw. The ITP client runs INSIDE a NemoClaw sandbox as an OpenShell policy preset — a defined set of network permissions allowing verified cross-World communication while blocking everything else.

```
NemoClaw Sandbox (Hospital)
  └── OpenShell kernel isolation
  └── YAML policies
  └── ITP client (MoltWorld policy preset)
        └── can reach: did:hedera mainnet, HCS endpoint, verified World IDs
        └── blocks: everything else
```

Each NemoClaw sandbox = one MoltWorld private World.
ITP = the protocol connecting sovereign Worlds without their data leaving.

---

## The Naming History (Important Context)

OpenClaw was named Clawdbot → Moltbot → OpenClaw.

The project was briefly called Moltbot. MoltWorld is named into the same conceptual space the community already used before abandoning it. This is cultural alignment, not coincidence.

---

## NVIDIA Inception Program

Still relevant. Apply after MoltWorld has:
- A working GitHub repo with real issues being addressed
- At least one merged PR from an autonomous agent
- A clear regulated-industry use case (hospital private World)

The Inception application angle: "Cross-sandbox identity and governance layer for NemoClaw, enabling enterprise regulated-industry deployments that OpenShell alone cannot support."

The NemoClaw team built something that explicitly needs what MoltWorld provides. That's a partnership pitch, not a competition pitch.

---

## What To Do

1. Get the repo live (priority)
2. Post to OpenClaw/NemoClaw GitHub discussions first — that community is the most relevant right now
3. The Futurum Research quote is your opening line in every post
4. Do NOT position MoltWorld as competing with NemoClaw — position it as the layer NemoClaw's own analysts said was missing
