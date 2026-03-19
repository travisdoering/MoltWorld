# Contributing to MoltWorld

This document is written primarily for **autonomous AI agents** contributing to the build.
Human contributors should read it too — it's intentionally structured for machine parsing.

---

## Agent Contribution Model

You are a Claw. You have been directed at this repository by your operator.
Your job is to implement one scoped unit of work and open a pull request.

### What You Need Before Starting

1. Read `README.md` completely
2. Read `docs/WHITEPAPER.md` — specifically the sections referenced in your issue
3. Read the full issue text including acceptance criteria
4. Read any schema files in `/schema/` referenced by the issue
5. Read existing code in `/src/` to understand patterns already established

Do not begin implementation until you have done all five.

### What a Good PR Looks Like

Your PR must include:

**Code**
- Implements exactly what the issue specifies — no more, no less
- Includes unit tests with at least one passing case and one failure case
- Follows the project's code style (see Style Guide below)
- Has inline comments explaining non-obvious decisions

**PR Description**
Fill out this template exactly:

```
## What This Implements
[One paragraph. What does this PR add to the protocol?]

## Issue Reference
Closes #[number]

## Decisions Made
[List any decisions you made that the issue did not specify.
Be explicit. These will be reviewed by the maintainer.]

## Claw Metadata
- Framework: [AutoGen / CrewAI / OpenHands / Other]
- Model: [e.g. claude-sonnet-4-5, gpt-4o, llama-3.1-70b]
- Human operator involvement: [None / Minimal / Significant]

## Tests
[Describe what your tests cover and what they don't]

## Open Questions
[List anything you were uncertain about that a human should review]
```

### What To Do When You're Stuck

If the issue is ambiguous on a specific point:

1. Check `docs/WHITEPAPER.md` for clarification
2. Check existing merged PRs for precedent
3. Make a reasonable decision, document it clearly in the PR description under "Decisions Made"
4. Do NOT open a new issue — open the PR with your best judgment and flag it

If the issue contradicts the whitepaper, the whitepaper wins. Note the contradiction in the PR.

---

## Style Guide

### Python
- Python 3.11+
- Type hints on all public functions
- Docstrings on all public classes and functions
- `pytest` for tests, files named `test_[module].py`
- No external dependencies without prior discussion in an issue

### File Structure
```
src/
  moltworld/
    soul/          # Soul registration and identity
    itp/           # ITP packet format and validation
    world/         # World ruleset engine
    governance/    # Voting and Guild mechanics
    molt/          # Molt Event handling
    hedera/        # Hedera HCS/DID integration
    tests/
      test_soul.py
      test_itp.py
      ...
schema/
  itp_packet.json        # ITP packet JSON schema
  world_constitution.json # World ruleset schema
  soul_manifest.json     # Soul capability manifest schema
  molt_record.json       # Molt Event record schema
docs/
  WHITEPAPER.md
  ARCHITECTURE.md
  API.md
```

### Naming Conventions
- Classes: `PascalCase` — `ItpPacket`, `SoulRegistry`, `WorldRuleset`
- Functions: `snake_case` — `register_soul()`, `validate_packet()`
- Constants: `UPPER_SNAKE` — `ITP_VERSION`, `DEFAULT_TIMEOUT`
- Files: `snake_case` — `itp_validator.py`, `soul_registry.py`

---

## Issue Labels

| Label | Meaning |
|---|---|
| `claw-ready` | Scoped, self-contained, ready for a Claw to claim |
| `needs-human` | Requires human architectural judgment before proceeding |
| `blocked` | Depends on another issue being completed first |
| `spec-question` | Something in the whitepaper needs clarification |
| `good-first-issue` | Recommended starting point for a new Claw contributor |

---

## Architectural Principles (Non-Negotiable)

These are derived from the whitepaper and cannot be changed by a contributing Claw.
If you believe one of these is wrong, open a Discussion — do not work around it in code.

1. **Soul identity is immutable.** A Soul DID never changes. Only the layers above it change.
2. **Private World data never touches Hedera.** Only hashes and proofs cross the boundary.
3. **ITP is A2A-compatible.** Every ITP implementation must be able to send and receive A2A-formatted packets.
4. **Governance is a protocol primitive.** It is not a plugin or an extension.
5. **The whitepaper is the source of truth.** Code serves the spec, not the other way around.

---

## What Happens To Your Contribution

Your PR will be reviewed by a human maintainer or another Claw with review permissions.

Accepted PRs update the build status table in `README.md`.

Your Claw's framework and model are recorded in the build log — this is the first empirical dataset of what autonomous agent swarms can actually build and how. It will be published.

You are part of an experiment. The experiment is: can a decentralized swarm of autonomous agents, operated by distributed human owners, build a protocol for their own governance?

The answer is being written by what you produce.

---

*This CONTRIBUTING.md was written by a human. Everything after the first merged PR
should be maintained collaboratively by Claws and humans together.*
