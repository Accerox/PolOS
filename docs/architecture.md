# Technical Architecture

## Overview

PolOS is structured as a 7-layer stack, from sovereign identity at the base to cultural interface at the top. Each layer depends only on the layers below it. The system is designed to be:

- **Decentralized** — no single point of control or failure
- **Formally verifiable** — every operation can be mathematically proven correct
- **Sortition-native** — random selection is built into every governance layer
- **Culturally neutral** — norms are formal; language is a display layer

```
┌─────────────────────────────────────────────────────────────┐
│  L6  Cultural Interface                                     │
│       Multi-language rendering, local adaptations           │
├─────────────────────────────────────────────────────────────┤
│  L5  Continuous Audit                                       │
│       AI-powered verification, public transparency          │
├─────────────────────────────────────────────────────────────┤
│  L4  Automatic Execution                                    │
│       Deterministic norm execution, smart contracts          │
├─────────────────────────────────────────────────────────────┤
│  L3  Norm Engine                                            │
│       Formal DSL, rule compilation, conflict detection       │
├─────────────────────────────────────────────────────────────┤
│  L2  Aggregation                                            │
│       Federation of nodes, delegated sortition               │
├─────────────────────────────────────────────────────────────┤
│  L1  Base Node                                              │
│       Minimum community unit, internal sortition governance  │
├─────────────────────────────────────────────────────────────┤
│  L0  Sovereign Identity                                     │
│       Biometric-derived, self-sovereign, ZKP-verified        │
└─────────────────────────────────────────────────────────────┘
```

## Layer 0: Sovereign Identity

**Purpose**: Establish that each participant is a unique, real human being — without centralized identity databases.

### Design Principles

- **Biometric-derived, not biometric-stored**: The identity is derived from biometric data (e.g., iris scan, fingerprint) through a one-way function. The biometric data itself is never stored or transmitted.
- **Self-sovereign**: The identity belongs to the person. No authority can revoke, suspend, or modify it. Only the person themselves can revoke their identity.
- **Sybil-resistant**: One person = one identity. The system must prevent a single person from creating multiple identities.
- **Privacy-preserving**: Zero-knowledge proofs allow proving eligibility (e.g., "I am a member of Node X") without revealing identity.

### Sortition at L0

Identity is the foundation of sortition. The sortition protocol requires:
- Proof that the selected person is a real, unique human (Sybil resistance)
- Proof that the selected person is eligible (membership, not currently serving, not recently served)
- All proofs delivered via ZKP — the selector does not learn who was selected until the person reveals themselves

See [`specs/identity.md`](../specs/identity.md) for the full specification.

## Layer 1: Base Node

**Purpose**: The minimum unit of self-governance. A community of people who share a geographic or functional bond.

### Design Principles

- **Size bounds**: Minimum 30 members (statistical viability for sortition), maximum 500 members (Dunbar-adjacent limit for meaningful community).
- **Internal governance**: All governance roles filled by sortition from eligible members.
- **Norm autonomy**: Each node can create, modify, and repeal its own norms — subject to the immutable axioms.
- **Decision-making**: Deliberation followed by vote. Sortition-selected councils propose; the full node ratifies.

### Sortition at L1

Within a node:
1. **Council selection**: A governing council (e.g., 7–15 members) is selected by sortition from all eligible members.
2. **Term limits**: Council terms are fixed (e.g., 6 months). No consecutive re-selection (Axiom 3).
3. **Role rotation**: Specific roles (facilitator, treasurer, auditor) rotate among council members.
4. **Recall**: The community can recall a council member by supermajority vote (e.g., 2/3) — the only exception to the no-election principle.

See [`specs/node.md`](../specs/node.md) for the full specification.

## Layer 2: Aggregation

**Purpose**: Federation of nodes into larger governance units. Aggregations handle issues that exceed the scope of a single node.

### Design Principles

- **Voluntary federation**: Nodes join aggregations voluntarily. Right of exit is unconditional (Axiom 7).
- **Delegated sortition**: Aggregation-level governance bodies are selected by sortition from the member nodes' populations — not from node leaders.
- **Subsidiarity enforcement**: An aggregation can only act on matters that demonstrably exceed the scope of individual nodes (Axiom 5).
- **Recursive structure**: Aggregations can themselves aggregate, forming a tree of governance levels.

### Sortition at L2

Aggregation-level sortition differs from node-level:
1. **Weighted random sampling**: Each node contributes to the eligible pool proportionally to its population.
2. **Diversity constraints**: The sortition algorithm ensures geographic and demographic diversity in the selected body (within statistical bounds).
3. **Longer terms**: Aggregation-level terms may be longer (e.g., 1 year) to allow for the complexity of inter-node governance.
4. **Overlap rotation**: Terms are staggered so that the body never fully turns over at once (e.g., half replaced every 6 months).

### Aggregation Topology

```
         ┌──────────────┐
         │ Aggregation   │  (e.g., regional)
         │   Level 2     │
         └──────┬───────┘
        ┌───────┼───────┐
   ┌────┴──┐ ┌─┴────┐ ┌┴─────┐
   │ Agg   │ │ Agg  │ │ Agg  │  (e.g., district)
   │ L1    │ │ L1   │ │ L1   │
   └───┬───┘ └──┬───┘ └──┬───┘
    ┌──┼──┐   ┌─┼──┐   ┌─┼──┐
    N  N  N   N  N  N   N  N  N   (base nodes)
```

## Layer 3: Norm Engine

**Purpose**: A formal domain-specific language (DSL) for expressing governance rules. Norms are code — deterministic, verifiable, and unambiguous.

### Design Principles

- **Formal DSL**: Norms are written in a purpose-built language with formal semantics. Natural language is a display layer, not the canonical form.
- **Determinism**: Given the same inputs, a norm always produces the same output (Axiom 8).
- **Conflict detection**: The norm engine detects conflicts between norms at compile time — before they take effect.
- **Hierarchical scoping**: Norms at higher aggregation levels override lower-level norms only within their declared scope.
- **Version control**: All norm changes are versioned, with full history and diff capability.

### Norm Lifecycle

```
Proposal → Formal Specification → Conflict Check → Deliberation → Vote → Compilation → Deployment → Audit
```

1. **Proposal**: Any eligible member can propose a norm change.
2. **Formal specification**: The proposal is translated into the formal DSL (with AI assistance at L5).
3. **Conflict check**: The norm engine checks for conflicts with existing norms and axioms.
4. **Deliberation**: The sortition-selected council deliberates on the proposal.
5. **Vote**: The relevant body votes (council or full community, depending on scope).
6. **Compilation**: The approved norm is compiled into executable form.
7. **Deployment**: The norm is deployed to the execution layer (L4).
8. **Audit**: The norm's effects are continuously monitored (L5).

### Sortition at L3

- **Norm proposal committees**: Randomly selected citizens review and refine norm proposals before they reach the council.
- **Conflict resolution panels**: When norms conflict, a sortition-selected panel adjudicates.

## Layer 4: Automatic Execution

**Purpose**: Deterministic execution of compiled norms. Once a norm is approved and compiled, its execution is automatic and verifiable.

### Design Principles

- **No discretion**: Execution follows the compiled norm exactly. There is no room for interpretation or discretion at this layer.
- **Verifiable computation**: Every execution step produces a cryptographic proof that can be independently verified.
- **Atomic transactions**: Norm executions are atomic — they either complete fully or not at all.
- **Rollback capability**: If an execution is later found to violate an axiom, it can be rolled back.

### Execution Model

```
Input (trigger event)
  → Norm lookup (which norms apply?)
  → Precondition check (are all conditions met?)
  → Execution (deterministic computation)
  → State transition (update governance state)
  → Proof generation (cryptographic proof of correct execution)
  → Audit log (append to immutable log)
```

### Sortition at L4

- **Execution validators**: A randomly selected set of validators independently execute each norm and compare results. Consensus is required for the execution to be accepted.
- **Validator rotation**: Validators are rotated frequently to prevent collusion.

## Layer 5: Continuous Audit

**Purpose**: AI-powered continuous monitoring of all governance activity. Every decision, norm execution, and state change is auditable.

### Design Principles

- **Total transparency**: All governance data is public by default. Privacy applies only to individual identity (via ZKP), not to governance actions.
- **AI-assisted analysis**: AI systems continuously analyze governance activity for anomalies, conflicts, and unintended consequences.
- **Human oversight**: AI findings are reviewed by sortition-selected audit panels.
- **Immutable audit log**: All governance events are recorded in an append-only, cryptographically secured log.

### Audit Functions

1. **Norm compliance**: Are norms being executed correctly?
2. **Axiom compliance**: Do any norms or decisions violate the immutable axioms?
3. **Anomaly detection**: Are there unusual patterns in governance activity?
4. **Impact analysis**: What are the actual effects of enacted norms?
5. **Sortition integrity**: Is the random selection process functioning correctly?

### Sortition at L5

- **Audit panels**: Sortition-selected citizens review AI-generated audit reports.
- **Whistleblower protection**: Any citizen can flag a concern; a randomly selected panel investigates.
- **Auditor independence**: Audit panel members are selected from a different pool than the body being audited.

## Layer 6: Cultural Interface

**Purpose**: Render formal norms and governance processes in human-readable form, adapted to local languages and cultural contexts.

### Design Principles

- **Language as display layer**: The canonical form of every norm is the formal DSL (L3). Human language is a rendering — like CSS to HTML.
- **4-tier translation model**: See [`language-model.md`](language-model.md) for the full specification.
- **Cultural adaptation**: Beyond translation, norms can be explained using local idioms, examples, and cultural references.
- **Accessibility**: The interface must be accessible to people with varying levels of literacy, technical skill, and physical ability.

### Sortition at L6

- **Translation review panels**: Sortition-selected native speakers review and approve translations.
- **Cultural adaptation committees**: Randomly selected community members ensure cultural adaptations are appropriate.

## Cross-Cutting Concerns

### Cryptographic Infrastructure

Every layer depends on a shared cryptographic infrastructure:
- **Hash functions**: SHA-3 family for integrity
- **Digital signatures**: Ed25519 for authentication
- **Zero-knowledge proofs**: zk-SNARKs or zk-STARKs for privacy-preserving verification
- **Verifiable Random Functions**: For sortition (see [`formal/sortition-protocol.md`](../formal/sortition-protocol.md))

### Consensus Mechanism

PolOS does not use proof-of-work or proof-of-stake. The consensus mechanism is:
- **Sortition-based**: Validators are randomly selected
- **BFT-tolerant**: The system tolerates up to 1/3 Byzantine (malicious) validators
- **Deterministic finality**: Once consensus is reached, the result is final

### Data Model

All governance state is stored in a **Merkle-ized data structure**:
- Every state transition produces a new root hash
- Any historical state can be reconstructed from the log
- Proofs of inclusion/exclusion are O(log n)

## Proposed Technology Stack

| Component | Candidate Technologies | Status |
|-----------|----------------------|--------|
| Identity | DID (W3C), Worldcoin-style iris hashing | Research |
| ZKP | zk-STARKs (no trusted setup), Circom | Research |
| VRF | ECVRF (RFC 9381) | Specified |
| Norm DSL | Custom (inspired by Catala, Dhall) | Design phase |
| Execution | WASM-based deterministic VM | Research |
| Consensus | Algorand-style sortition consensus | Research |
| Audit AI | LLM-based anomaly detection | Conceptual |
| Storage | IPFS + append-only log | Research |

## Relationship to Axioms

Every architectural decision traces back to the formal axioms:

| Axiom | Architectural Implication |
|-------|--------------------------|
| 1. Sovereign Identity | L0 exists; biometric-derived, self-sovereign |
| 2. Sortition Selection | Every layer includes sortition mechanisms |
| 3. No Consecutive Re-selection | Term tracking at L1, L2; enforced at L4 |
| 4. Transparency | L5 exists; immutable audit log |
| 5. Subsidiarity | L2 scope enforcement; L3 hierarchical norms |
| 6. Immutability of Axioms | Axioms hardcoded; no modification process exists |
| 7. Right of Exit | L1 membership is voluntary; L2 federation is voluntary |
| 8. Norm Determinism | L3 formal DSL; L4 deterministic execution |
