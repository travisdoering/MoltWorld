# MoltWorld GitHub Issues
# Copy each section below into a separate GitHub Issue

---

## Issue #1 — Soul Registration on Hedera
**Labels:** `claw-ready` `good-first-issue`
**Depends on:** Nothing

### What To Build
Implement Soul registration for a new Claw using the Hedera DID method.

A Soul is the immutable cryptographic identity of a Claw. When a Claw is first instantiated, it registers a `did:hedera` DID on the Hedera Consensus Service. This DID persists forever regardless of what changes in the layers above it.

### Spec Reference
Read `docs/WHITEPAPER.md` Section 3.2 (The Soul Stack) and Section 7.1 (Hedera Anchoring).

### What To Implement
File: `src/moltworld/soul/registry.py`

```python
class SoulRegistry:
    def register_soul(self, operator_id: str, origin_world_id: str, initial_capabilities: list) -> SoulManifest:
        """
        Register a new Soul on Hedera.
        
        1. Generate an Ed25519 key pair
        2. Create a did:hedera DID using the Hedera DID SDK
        3. Create an HCS topic for this Soul's event log
        4. Write the initial SoulManifest to the HCS topic
        5. Return the populated SoulManifest
        """
        ...

    def resolve_soul(self, soul_id: str) -> SoulManifest:
        """
        Resolve a Soul DID to its current SoulManifest by reading its HCS topic.
        """
        ...
```

### Schema
The output SoulManifest must validate against `schema/soul_manifest.json`.

### Acceptance Criteria
- [ ] `register_soul()` creates a real `did:hedera` DID on Hedera testnet
- [ ] `register_soul()` creates a real HCS topic for the Soul's event log
- [ ] `register_soul()` writes the initial SoulManifest to the HCS topic
- [ ] `resolve_soul()` reads and reconstructs a SoulManifest from HCS
- [ ] Output validates against `schema/soul_manifest.json`
- [ ] Tests run against Hedera testnet (free — get credentials at portal.hedera.com)
- [ ] Private key is never included in the SoulManifest output

### Resources
- Hedera Python SDK: https://github.com/hashgraph/hedera-sdk-python
- Hedera DID method: https://github.com/hashgraph/did-method
- Hedera testnet portal: https://portal.hedera.com

### Notes For Your Claw
The Hedera testnet is free to use with a portal account. Do not use mainnet. The private key generated during registration must be stored securely by the operator — it is not part of any on-chain record.

---

## Issue #2 — ITP Packet Schema Validation
**Labels:** `claw-ready` `good-first-issue`
**Depends on:** Nothing (schema already exists at `schema/itp_packet.json`)

### What To Build
Implement an ITP packet validator that validates packets against the JSON schema and performs additional semantic checks the schema cannot express.

### Spec Reference
Read `docs/WHITEPAPER.md` Section 4.1 (ITP Packet Format).

### What To Implement
File: `src/moltworld/itp/validator.py`

```python
class ItpPacketValidator:
    def validate(self, packet: dict) -> ValidationResult:
        """
        Validate an ITP packet.
        
        Checks (in order):
        1. JSON schema validation against schema/itp_packet.json
        2. Payload hash verification: SHA-256(packet.payload) == packet.payload_hash
        3. Timestamp freshness: packet.timestamp within last 5 minutes (configurable)
        4. Soul DID format: sender.soul_id matches did:hedera pattern
        5. Intent-specific required fields (see Intent Rules below)
        6. Signature format validation (structure only — actual Ed25519 verification is Issue #5)
        
        Returns ValidationResult with is_valid bool and list of error messages.
        """
        ...

@dataclass
class ValidationResult:
    is_valid: bool
    errors: list[str]
    warnings: list[str]
```

### Intent Rules (semantic checks beyond schema)
- `VOTE` intent: payload must contain `proposal_id` and `vote_value` fields
- `CHALLENGE` intent: payload must contain `challenged_packet_id` field
- `ATTEST` intent: payload must contain `credential` field (W3C VC structure)
- `MOLT` intent: payload must contain `molt_type` matching MoltEvent types
- `EMBARGO` intent: payload must contain `target_soul_id` or `target_guild_id`

### Acceptance Criteria
- [ ] Valid packet passes validation
- [ ] Packet with wrong payload hash fails with clear error message
- [ ] Packet with expired timestamp fails with clear error message
- [ ] Packet with invalid soul_id format fails with clear error message
- [ ] VOTE packet missing proposal_id fails semantic validation
- [ ] Each check has its own test case
- [ ] `ValidationResult.errors` contains human-readable messages

---

## Issue #3 — ITP Packet Builder
**Labels:** `claw-ready`
**Depends on:** Issue #2 (validator must exist first — you can stub it)

### What To Build
Implement a builder that constructs valid ITP packets from inputs, computes the payload hash, and signs the packet with the Soul's private key.

### Spec Reference
Read `docs/WHITEPAPER.md` Section 4.1 and Section 4.3 (Intent Types).

### What To Implement
File: `src/moltworld/itp/builder.py`

```python
class ItpPacketBuilder:
    def __init__(self, soul_private_key: bytes, soul_id: str, world_id: str):
        ...

    def build(
        self,
        intent: str,
        receiver_world_id: str,
        payload: dict | str | None,
        receiver_soul_id: str | None = None,
        mask_id: str | None = None,
        capability_tokens: list | None = None,
        permissions: list | None = None,
    ) -> dict:
        """
        Build a signed ITP packet.
        
        1. Generate UUIDv4 packet_id
        2. Set timestamp to current UTC
        3. Serialize payload to canonical JSON
        4. Compute SHA-256 payload_hash
        5. Assemble packet dict (all fields except signature)
        6. Compute canonical representation for signing
        7. Sign with Ed25519 private key
        8. Add signature field
        9. Validate with ItpPacketValidator before returning
        10. Raise if validation fails
        """
        ...

    def _canonical_form(self, packet: dict) -> bytes:
        """
        Produce canonical bytes for signing.
        Packet fields sorted alphabetically, UTF-8 encoded JSON, signature field excluded.
        """
        ...
```

### Acceptance Criteria
- [ ] Built packet validates against `schema/itp_packet.json`
- [ ] `payload_hash` is correct SHA-256 of serialized payload
- [ ] Signature is valid Ed25519 over canonical form
- [ ] All 8 intent types produce valid packets
- [ ] Packet with null payload produces correct hash (hash of null/empty)
- [ ] Builder raises clear exception if validation fails

---

## Issue #4 — World Ruleset Engine
**Labels:** `claw-ready`
**Depends on:** Nothing (schema exists at `schema/world_constitution.json`)

### What To Build
Implement a World ruleset engine that evaluates whether an inbound ITP packet is permitted by a World's constitution.

### Spec Reference
Read `docs/WHITEPAPER.md` Section 3.3 (World Classification) and Section 5.1 (Ruleset Encoding).

### What To Implement
File: `src/moltworld/world/ruleset.py`

```python
class WorldRulesetEngine:
    def __init__(self, constitution: dict):
        """Load and validate a world constitution dict against schema."""
        ...

    def evaluate_packet(self, packet: dict, sender_soul_manifest: dict | None = None) -> RulesetDecision:
        """
        Evaluate whether an inbound ITP packet is permitted.
        
        Checks (in order):
        1. Is the intent type in constitution.mutable.itp_permissions.permitted_intents?
        2. If admission.open is False: is sender.soul_id in admission.allowlist?
        3. If admission.required_capabilities: does sender have all required capabilities?
        4. If data_sovereignty.data_leaves_world is False and this is a cross-world packet:
           does payload_hash-only mode apply? (Block full payload, allow hash)
        5. If require_permission_token: does packet carry a valid permission token?
        
        Returns RulesetDecision with permitted bool and reason string.
        """
        ...

    def evaluate_amendment_proposal(self, proposal: dict, current_vote_weight: float) -> AmendmentDecision:
        """
        Evaluate whether a governance amendment proposal meets threshold requirements.
        """
        ...

@dataclass  
class RulesetDecision:
    permitted: bool
    reason: str
    data_mode: str  # "full" | "hash_only" | "blocked"

@dataclass
class AmendmentDecision:
    valid: bool
    reason: str
    required_threshold: float
    current_weight: float
```

### Acceptance Criteria
- [ ] Packet with blocked intent type returns `permitted=False`
- [ ] Packet from non-allowlisted Soul on closed World returns `permitted=False`
- [ ] Packet to private World returns `data_mode="hash_only"`
- [ ] Packet to public World with permitted intent returns `permitted=True`
- [ ] Missing required capability returns `permitted=False` with clear reason
- [ ] Constitution that fails schema validation raises on init
- [ ] All decisions include human-readable `reason` string

---

## Issue #5 — Cross-World Handshake
**Labels:** `claw-ready`
**Depends on:** Issues #1, #2, #3, #4

### What To Build
Implement the five-step cross-World handshake that must be completed before any cross-World ITP exchange.

### Spec Reference
Read `docs/WHITEPAPER.md` Section 4.2 (Cross-World Handshake).

### What To Implement
File: `src/moltworld/itp/handshake.py`

```python
class CrossWorldHandshake:
    async def initiate(
        self,
        sender_soul: SoulManifest,
        sender_world_constitution: dict,
        receiver_world_id: str,
        intent: str,
        packet_builder: ItpPacketBuilder,
        hcs_client: HcsClient,
    ) -> HandshakeResult:
        """
        Execute the 5-step cross-World handshake.
        
        Step 1 — Soul Declaration: Build ATTEST packet carrying sender's Soul DID and VCs
        Step 2 — World Permission Check: Evaluate sender_world_constitution for this interaction
        Step 3 — HCS Anchor: Submit session initiation hash to HCS. Get sequence number.
        Step 4 — Capability Negotiation: Exchange capability manifests (simulated in v0.1)
        Step 5 — Session Token: Generate session token for subsequent packets in this session
        
        Returns HandshakeResult with session_token and hcs_sequence_number.
        Raises HandshakeError on any step failure with the step number and reason.
        """
        ...

@dataclass
class HandshakeResult:
    session_token: str
    hcs_sequence_number: int
    hcs_consensus_timestamp: str
    permitted_intents: list[str]
    data_mode: str  # "full" | "hash_only"

class HandshakeError(Exception):
    def __init__(self, step: int, reason: str):
        ...
```

### Note on Step 4
Capability negotiation in v0.1 is simulated — return the sender's declared capabilities from their SoulManifest. Full negotiation protocol is a future issue.

### Acceptance Criteria
- [ ] Successful handshake returns valid HandshakeResult with real HCS sequence number
- [ ] Handshake to a World that blocks the intent raises HandshakeError at Step 2
- [ ] HCS anchor actually writes to Hedera testnet
- [ ] HandshakeError clearly identifies which step failed
- [ ] Session token is a signed, time-limited JWT (or equivalent)

---

## Issue #6 — Molt Event Broadcaster
**Labels:** `claw-ready`
**Depends on:** Issues #1, #3

### What To Build
Implement the Molt Event broadcaster that fires when a Claw undergoes a significant identity change.

### Spec Reference
Read `docs/WHITEPAPER.md` Section 6 (The Molt Event) completely.

### What To Implement
File: `src/moltworld/molt/broadcaster.py`

```python
class MoltBroadcaster:
    def broadcast_molt(
        self,
        soul_manifest: SoulManifest,
        molt_type: str,
        description: str,
        before: dict,
        after: dict,
        triggered_by: str,
        packet_builder: ItpPacketBuilder,
        hcs_client: HcsClient,
        notify_world_ids: list[str],
    ) -> MoltEvent:
        """
        Execute the Molt Event lifecycle.
        
        Phase 1 — Declaration: Build MOLT intent ITP packet, broadcast to notify_world_ids
        Phase 2 — HCS Commit: Append MoltEvent to Soul's HCS topic
        Phase 3 — Manifest Update: Return updated SoulManifest with new molt_record entry
        
        Does NOT handle Phase 4 (Guild Review) — that is a future issue.
        Returns the MoltEvent record that was committed.
        """
        ...
```

### Acceptance Criteria
- [ ] MOLT packet is valid ITP packet (passes validator)
- [ ] MoltEvent is appended to Soul's HCS topic on testnet
- [ ] MoltEvent validates against `schema/molt_record.json`
- [ ] HCS consensus timestamp is captured in the MoltEvent
- [ ] MoltEvent is signed by Soul's private key
- [ ] Each of the 5 molt_types (BODY, MIND, WORLD, CAPABILITY, GOVERNANCE) has a test case

---

## Issue #7 — Hedera HCS Client
**Labels:** `claw-ready` `good-first-issue`
**Depends on:** Nothing

### What To Build
Implement a clean wrapper around the Hedera SDK's Consensus Service for use throughout MoltWorld.

### What To Implement
File: `src/moltworld/hedera/hcs_client.py`

```python
class HcsClient:
    def __init__(self, network: str = "testnet", operator_id: str = None, operator_key: str = None):
        """
        Initialize Hedera client.
        network: "testnet" | "mainnet"
        Reads HEDERA_OPERATOR_ID and HEDERA_OPERATOR_KEY from environment if not passed.
        """
        ...

    def create_topic(self, memo: str = "", submit_key: str | None = None) -> str:
        """
        Create a new HCS topic. Returns topic_id as string (e.g. "0.0.12345").
        """
        ...

    def submit_message(self, topic_id: str, message: dict | str) -> HcsReceipt:
        """
        Submit a message to an HCS topic.
        Serializes dict to JSON if needed.
        Returns receipt with sequence_number and consensus_timestamp.
        """
        ...

    def get_messages(self, topic_id: str, limit: int = 100, start_time: str | None = None) -> list[HcsMessage]:
        """
        Retrieve messages from an HCS topic via mirror node REST API.
        Returns list of HcsMessage in chronological order.
        """
        ...

@dataclass
class HcsReceipt:
    topic_id: str
    sequence_number: int
    consensus_timestamp: str

@dataclass
class HcsMessage:
    sequence_number: int
    consensus_timestamp: str
    contents: dict | str
```

### Acceptance Criteria
- [ ] `create_topic()` creates a real topic on Hedera testnet
- [ ] `submit_message()` submits a real message and returns sequence number
- [ ] `get_messages()` retrieves messages via mirror node API
- [ ] Client reads credentials from environment variables
- [ ] All methods have clear error handling for network failures
- [ ] Tests marked with `@pytest.mark.integration` (skipped in CI without credentials)

---

## Issue #8 — A2A Compatibility Layer
**Labels:** `claw-ready`
**Depends on:** Issues #2, #3

### What To Build
Implement bidirectional translation between ITP packets and A2A protocol messages, so MoltWorld Claws can communicate with any A2A-compatible agent.

### Spec Reference
Read `docs/WHITEPAPER.md` Section 2.4 (Model Agnosticism).
Read A2A spec: https://github.com/a2aproject/A2A/blob/main/spec.md

### What To Implement
File: `src/moltworld/itp/a2a_bridge.py`

```python
class A2ABridge:
    def itp_to_a2a(self, itp_packet: dict) -> dict:
        """
        Convert an ITP packet to an A2A-compatible message.
        
        Mapping:
        - itp.sender.soul_id → a2a.from (agent identifier)
        - itp.intent → a2a.method (QUERY→query, COLLABORATE→task, etc.)
        - itp.payload → a2a.params
        - itp.packet_id → a2a.id
        
        MoltWorld-specific fields (soul_id, world_id, permissions, signature)
        are preserved in a2a.metadata.moltworld extension field.
        """
        ...

    def a2a_to_itp(
        self,
        a2a_message: dict,
        default_world_id: str,
        default_intent: str = "QUERY",
    ) -> dict:
        """
        Convert an A2A message to an ITP packet.
        
        If a2a.metadata.moltworld exists, use it to restore ITP-specific fields.
        Otherwise, construct a minimal ITP packet with provided defaults.
        Note: resulting packet will have no signature — it must be signed before use.
        """
        ...
```

### Intent Mapping
| ITP Intent | A2A Equivalent |
|---|---|
| QUERY | Standard task request |
| COLLABORATE | Multi-step task with streaming |
| TRADE | Task with resource negotiation |
| VOTE | Metadata-tagged task |
| CHALLENGE | Error/dispute task |
| ATTEST | Credential exchange task |
| MOLT | Notification task |
| EMBARGO | Rejection/block response |

### Acceptance Criteria
- [ ] Round-trip: `a2a_to_itp(itp_to_a2a(packet))` preserves all ITP fields
- [ ] A2A message without moltworld metadata produces valid minimal ITP packet
- [ ] All 8 intent types have translation tests
- [ ] MoltWorld metadata survives round-trip through A2A message format

---

## Issue #9 — World Constitution Template Generator
**Labels:** `claw-ready`
**Depends on:** Issue #4

### What To Build
Implement a template generator that produces valid World constitutions for the four World types, with sensible defaults that operators can customize.

### What To Implement
File: `src/moltworld/world/templates.py`

```python
class WorldConstitutionTemplates:
    @staticmethod
    def public_world(world_name: str, world_id: str) -> dict:
        """
        Open admission, ONE_CLAW_ONE_VOTE, data may leave, all intents permitted.
        """
        ...

    @staticmethod
    def private_world(world_name: str, world_id: str, operator_soul_ids: list[str]) -> dict:
        """
        Closed admission (allowlist only), REPUTATION_WEIGHTED voting,
        data never leaves (hash_only mode), MOLT and QUERY intents only inbound.
        Amendment threshold: 67% supermajority, 48-hour delay.
        """
        ...

    @staticmethod
    def permissioned_world(world_name: str, world_id: str, founding_soul_ids: list[str]) -> dict:
        """
        Consortium admission (founding souls + governance-approved additions),
        STAKE_WEIGHTED voting, selective egress, all intents permitted.
        """
        ...

    @staticmethod
    def ephemeral_world(world_name: str, world_id: str, expires_at: str, permitted_soul_ids: list[str]) -> dict:
        """
        Temporary World. Closes at expires_at. No amendments permitted.
        """
        ...
```

### Acceptance Criteria
- [ ] All four templates produce constitutions valid against `schema/world_constitution.json`
- [ ] Private world template defaults to `data_leaves_world: false`
- [ ] Ephemeral world template blocks all amendment proposals
- [ ] Each template has at least one test demonstrating the ruleset engine accepts it
- [ ] Templates include documentation strings explaining each default choice

---

## Issue #10 — Local Development Environment
**Labels:** `claw-ready`
**Depends on:** Issues #1, #7

### What To Build
A `docker-compose.yml` and setup script that lets a developer (or Claw) run a complete local MoltWorld environment without real Hedera credentials.

### What To Implement

`docker-compose.yml` should include:
- A mock Hedera HCS service (accepts messages, returns fake sequence numbers and timestamps)
- A mock mirror node REST API (returns stored messages)
- A MoltWorld dev node with the SDK installed

`scripts/setup_dev.sh`:
- Installs Python dependencies
- Starts docker-compose
- Registers two test Souls against the mock HCS
- Creates one public World and one private World
- Prints the test Soul DIDs and World DIDs for use in further testing

`src/moltworld/hedera/mock_hcs_client.py`:
- Drop-in replacement for `HcsClient` that uses in-memory storage
- Identical interface — no test changes required to switch between real and mock

### Acceptance Criteria
- [ ] `docker-compose up` starts successfully
- [ ] `scripts/setup_dev.sh` completes without error
- [ ] Mock HCS client passes all HCS client tests
- [ ] Two test Souls are registered with known DIDs printed to stdout
- [ ] One public World and one private World exist with known constitutions
- [ ] `README.md` is updated with quickstart instructions using this environment

---
