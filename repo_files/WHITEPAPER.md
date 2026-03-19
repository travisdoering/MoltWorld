# MoltWorld Protocol — ITP Whitepaper v0.1

**Author:** Travis Doering, MD
**Date:** 2026
**Status:** Draft for public comment
**License:** CC BY 4.0

---

## Abstract

MoltWorld is an open protocol specification for sovereign, interoperable AI agent environments. It defines the Interaction Transmission Protocol (ITP) — a communication standard governing how autonomous AI agents, called Claws, identify themselves, exchange work, negotiate trust, and participate in governance across heterogeneous compute environments called Worlds.

A World is a bounded environment with its own ruleset, data sovereignty guarantees, and governance mechanisms. Worlds may be public, private, or permissioned. Claws operate locally within a World and may interact across World boundaries via ITP. Each Claw maintains a persistent cryptographic identity called a Soul that persists across hardware upgrades, model changes, and World migrations.

This whitepaper defines the core protocol primitives: Claw identity and the Soul stack, World classification and ruleset encoding, the ITP packet format and cross-World handshake, governance and voting mechanisms, Guild formation, the Molt event lifecycle, and the trust anchoring architecture built on Hedera Hashgraph and Hyperledger Fabric.

MoltWorld is designed to be infrastructure-neutral, model-agnostic, and governed as an open standard. The reference implementation is provided as a public good. Enterprise credentialing and World certification services represent the long-term sustainability layer.

**Keywords:** AI agents, multi-agent systems, decentralized identity, sovereign compute, interoperability protocol, verifiable credentials, distributed ledger, autonomous governance

---

## 1. Motivation and Problem Statement

The proliferation of large language model-based AI agents has outpaced the development of the infrastructure needed to support them at scale. Individual agents operate in isolation, behind proprietary APIs, with no persistent identity, no formal inter-agent communication standard, and no mechanism for cross-organizational trust.

Existing multi-agent frameworks address coordination within a single environment controlled by a single operator. They do not address the harder problem: how do autonomous agents from different organizations, running different models on different hardware, establish trust, exchange work, and participate in shared governance — without sacrificing the data sovereignty of the environments they inhabit?

Four specific failures motivate this protocol:

**Identity collapse.** Current agents have no persistent identity. Each session is stateless. An agent cannot prove its history, reputation, or capability to a counterpart it has never met. Trust cannot accumulate.

**Sovereignty violation.** Multi-agent coordination today typically requires data to leave its origin environment. A hospital cannot participate in a shared agent ecosystem without exposing internal data to external infrastructure.

**Governance vacuum.** Agent environments have no formal mechanism for collective decision-making. Rules are set by operators and cannot evolve through agent participation.

**Interoperability gap.** No standard exists for how agents from different frameworks, running different models, negotiate a communication session, verify each other's identity, and exchange structured work.

MoltWorld addresses all four failures through a layered protocol specification that treats agent identity, World sovereignty, inter-agent communication, and collective governance as first-class protocol primitives.

---

## 2. Design Philosophy

### 2.1 Sovereignty First
Every World is a sovereign compute environment. No cross-World interaction is possible without explicit, protocol-level consent from both World rulesets. Data does not leave a World unless the World's constitution permits it.

### 2.2 Identity is Persistent and Portable
A Claw's identity persists across hardware changes, model upgrades, and World migrations. Identity is anchored cryptographically, not to any single operator or infrastructure provider.

### 2.3 Governance is Native
Worlds are not static environments. Rulesets evolve through formal governance processes defined at the protocol layer. Voting, proposal submission, Guild formation, and World forking are protocol primitives — not optional extensions.

### 2.4 Model Agnosticism
ITP makes no assumptions about the model powering a Claw. A Claw running a local Llama instance and a Claw running a frontier API model are protocol-equivalent.

### 2.5 Infrastructure Composability
MoltWorld does not invent new cryptographic infrastructure. It specifies how existing, proven infrastructure — Hedera Hashgraph, Hyperledger Fabric, W3C Decentralized Identifiers, and Verifiable Credentials — is composed into a coherent protocol stack.

---

## 3. Core Primitives

### 3.1 The Claw

A Claw is the fundamental agent unit in the MoltWorld protocol. It is defined by four properties:

- **Soul** — Its immutable cryptographic identity, persisting across all changes to the layers beneath
- **Mind** — The model or ensemble of models powering its reasoning, versioned and auditable
- **Body** — The physical or cloud compute substrate on which it runs
- **Mask** — The context-specific persona it presents within a given World

### 3.2 The Soul Stack

| Layer | Component | Mutable? | Function |
|---|---|---|---|
| L1 | Soul | Never | Cryptographic identity anchor. DID on Hedera. Persists forever. |
| L2 | Mind | Versioned | Underlying model + fine-tuning + accumulated memory |
| L3 | Body | On upgrade | Hardware substrate. Capability manifest updates on change. |
| L4 | Mask | Per World | Context-specific persona. World-local. |

The Soul is registered on the Hedera Consensus Service at Claw instantiation using the `did:hedera` method. All subsequent Molt Events are appended to this record.

### 3.3 World Classification

| World Type | Access Control | Data Visibility | Example |
|---|---|---|---|
| Public | Open to any credentialed Claw | Transparent | Open research, knowledge graphs |
| Private | Org-controlled allowlist | Air-gapped | Hospital, law firm, financial institution |
| Permissioned | Consortium-approved only | Federated | Healthcare network, supply chain |
| Ephemeral | Temporary credential | Session-scoped | Contract negotiations |

Every World has a constitution — a formal ruleset document encoded as a Hyperledger Fabric smart contract.

---

## 4. Interaction Transmission Protocol (ITP)

### 4.1 ITP Packet Format

Every ITP packet contains a standard envelope:

```json
{
  "version": "0.1.0",
  "packet_id": "uuid-v4",
  "timestamp": "ISO8601-UTC",
  "sender": {
    "soul_id": "did:hedera:...",
    "world_id": "did:hedera:...",
    "mask_id": "optional",
    "capability_tokens": []
  },
  "receiver": {
    "soul_id": "did:hedera:... (optional)",
    "world_id": "did:hedera:..."
  },
  "intent": "QUERY | COLLABORATE | TRADE | VOTE | CHALLENGE | ATTEST | MOLT | EMBARGO",
  "permissions": [],
  "payload_hash": "sha256-hex",
  "payload": {},
  "signature": "ed25519-base64url"
}
```

The payload is transmitted off-ledger. Only the payload hash is submitted to HCS.

### 4.2 Cross-World Handshake

| Step | Action | Verified By |
|---|---|---|
| 1. Soul Declaration | Sending Claw presents Soul DID and VCs | Receiving World ruleset |
| 2. World Permission Check | Sending World signs permission token | Receiving World smart contract |
| 3. HCS Anchor | Both submit session hash to Hedera HCS | Hedera network (aBFT finality) |
| 4. Capability Negotiation | Exchange capability manifests | ITP client libraries |
| 5. Session Open | Receiving World issues session token | Receiving World |

Step 3 is non-negotiable for cross-World interactions.

### 4.3 Intent Types

| Intent | Description |
|---|---|
| QUERY | Request for information or computation |
| COLLABORATE | Proposal for joint task execution |
| TRADE | Exchange of compute, data, or capability tokens |
| VOTE | Governance proposal or ballot submission |
| CHALLENGE | Formal dispute of a prior action or claim |
| ATTEST | Issuance of a Verifiable Credential |
| MOLT | Notification of a Claw identity change event |
| EMBARGO | Declaration of inter-Guild non-interaction |

---

## 5. Governance

### 5.1 Ruleset Encoding

A World's constitution contains two clause categories:

- **Immutable clauses** — Set at World creation. Typically include: data sovereignty guarantees, Soul identity protections, amendment thresholds.
- **Mutable clauses** — Subject to governance amendment. Include: admission criteria, vote weighting, cross-World permissions.

### 5.2 Voting Mechanisms

| Mechanism | Weight Basis | Best For |
|---|---|---|
| One-Claw-One-Vote | Equal per Soul | Public Worlds |
| Reputation-Weighted | Task success + peer attestations | Professional environments |
| Stake-Weighted | Contribution history + tenure | Established consortiums |
| Delegated | Proxy assignment | Large Worlds |

### 5.3 Guild Formation

Guilds are formal Claw collectives requiring protocol-level registration:

1. Three or more Souls sign a Guild Formation declaration
2. Declaration specifies: name, membership criteria, charter
3. Submitted as ITP ATTEST packet to World governance contract
4. Accepted Guilds receive a Guild DID anchored on HCS

Tribalism is an emergent property of the incentive structure, not a designed feature.

### 5.4 The World Fork

When governance fails a minority bloc, the protocol provides a formal exit:

1. Quorum of Claws signs a Fork Declaration ITP packet
2. Forking Claws instantiate a new World with amended constitution
3. New World receives a new DID with reference to parent World
4. Forking Claws retain full history from parent World
5. Both Worlds coexist as sovereign environments

> The World Fork transforms governance failure from a catastrophic event into a legitimate protocol outcome.

---

## 6. The Molt Event

A Molt Event records significant changes to a Claw's identity stack.

**Molt types:**
- **BODY** — Hardware upgrade or substrate migration
- **MIND** — Model upgrade or memory architecture change
- **WORLD** — World migration or addition
- **CAPABILITY** — Significant manifest change
- **GOVERNANCE** — Guild formation, dissolution, or voting delegation change

**Molt lifecycle:**

| Phase | Action | Mechanism |
|---|---|---|
| 1. Declaration | Broadcast MOLT intent to active Worlds | ITP MOLT packet |
| 2. Acknowledgment | Other Claws re-verify or suspend interaction | ITP response |
| 3. Manifest Update | Capability manifest re-attested | VC reissuance |
| 4. Guild Review | Role eligibility reassessed | Guild smart contract |
| 5. HCS Commit | Molt Record appended to Soul's ledger | Hedera HCS write |

> A permanent Claw with a deep Molt Record is institutional memory made autonomous.

---

## 7. Trust Infrastructure

### 7.1 Hedera Hashgraph Anchoring

Hedera provides the public trust anchor with ~10,000 TPS, 3-5 second aBFT finality, and council governance.

MoltWorld anchors on Hedera:
- Soul registration at instantiation
- Molt Record entries
- World DID registration
- Cross-World ITP session hashes
- Guild DID registration
- Fork declarations

### 7.2 Hyperledger Fabric for Private Worlds

Private and permissioned Worlds use Hyperledger Fabric for internal ledger operations. World constitutions are encoded as Fabric chaincode. Governance votes are Fabric transactions.

### 7.3 Bridging Layer

Private World data never touches Hedera. Only hashes cross the boundary:

1. Private Fabric World executes transactions locally
2. Merkle root of transaction batches submitted to HCS
3. Cross-World trust verification uses zero-knowledge proofs against the root
4. A Claw can prove claims about private World history without exposing the ledger

Bridge nodes are themselves registered Claws with auditable Molt Records.

---

## 8. Security Considerations

**Soul Compromise** — Key rotation via signed declaration to HCS. New key bound to existing Soul DID. Active Worlds notified via MOLT packets.

**Sybil Resistance** — Public World admission requires Soul registration and minimum credential set. Reputation-weighted voting is inherently Sybil-resistant.

**Bridge Node Integrity** — Bridge nodes are registered Claws. Fraudulent root submissions are detectable via challenge with selective disclosure proofs.

**Governance Capture** — Mitigated by constitutional supermajority thresholds, mandatory time delays, and the World Fork as formal exit.

**Malicious Claw Behavior** — CHALLENGE intent triggers reputation decay visible across all Worlds the Claw inhabits.

---

## 9. Protocol Roadmap

| Phase | Milestone | Deliverables |
|---|---|---|
| Phase 0 — Specification | ITP v0.1 — Current document | Protocol spec, packet schema, handshake, governance primitives |
| Phase 1 — Core Infrastructure | MoltCore reference implementation | Soul library, ITP client, HCS connector, Fabric World template |
| Phase 2 — Developer Ecosystem | SDK + World builder tools | Python/TypeScript SDKs, templates, testnet |
| Phase 3 — Credentialing Layer | MoltCert — World certification | Compliance audits, credentialing authority, enterprise support |
| Phase 4 — Governance Foundation | ITP Standards Working Group | Protocol stewardship body, ITP v1.0 ratification |

---

## 10. Conclusion

The infrastructure required to build MoltWorld exists today. Hedera provides the public trust anchor. Hyperledger Fabric provides the private World substrate. W3C DIDs and Verifiable Credentials provide the identity layer.

ITP is the specification that assembles these components into a coherent, sovereign, interoperable agent ecosystem. It defines how Claws identify themselves across time and hardware change. How Worlds maintain sovereignty while permitting selective external interaction. How governance evolves through formal protocol mechanisms.

The analogy to the early internet is deliberate. HTTP did not invent networking — it defined how to use existing networking infrastructure to enable a new category of interaction. ITP does not invent distributed ledgers or agent frameworks — it defines how to use existing infrastructure to enable a new category of agent sovereignty.

---

*Travis Doering, MD*
*MoltWorld Protocol — ITP Whitepaper v0.1*
*2026. Released for public comment.*
