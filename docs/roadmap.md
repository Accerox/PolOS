# Project Roadmap

## Vision

PolOS aims to provide a complete, formally specified, and practically implementable governance system based on sortition, cryptographic verification, and decentralization. The roadmap spans from foundational specification to real-world deployment.

## Phase 0: Foundation (Current Phase)

**Duration**: 6–12 months
**Status**: In progress

### Objectives

- [x] Publish formal axiom specification
- [x] Publish sortition protocol specification
- [x] Publish philosophical foundation document
- [x] Publish technical architecture document
- [x] Publish L0 (Identity) and L1 (Node) specifications
- [x] Publish language model specification
- [x] Catalog open problems
- [ ] Establish community governance for the project itself (using sortition)
- [ ] Formal verification of axiom consistency and independence
- [ ] Peer review of specifications by domain experts (cryptography, political science, formal methods)
- [ ] Create project website and communication channels

### Deliverables

- This repository with all specification documents
- Formal proofs of axiom properties (consistency, independence, minimality)
- Community charter for the PolOS project

### Success Criteria

- Specifications reviewed by at least 3 independent domain experts
- No unresolved contradictions in the axiom system
- Active community of 50+ contributors

---

## Phase 1: Identity Prototype (L0)

**Duration**: 12–18 months
**Dependencies**: Phase 0 complete

### Objectives

- [ ] Design and implement a sovereign identity prototype
- [ ] Implement biometric-derived key generation (proof of concept)
- [ ] Implement zero-knowledge proof of identity membership
- [ ] Implement Sybil resistance mechanism (at least one approach)
- [ ] Security audit of identity system
- [ ] User testing with 100+ participants

### Technical Milestones

1. **Identity key derivation**: Implement biometric → key derivation with at least one biometric modality
2. **ZKP circuit**: Implement a ZKP circuit for "I am a member of set S" without revealing which member
3. **Uniqueness verification**: Implement a mechanism to prevent duplicate identities
4. **Identity SDK**: Publish an SDK for identity creation, verification, and ZKP generation

### Research Questions to Resolve

- Which biometric modality is most practical? (Iris? Fingerprint? Face?)
- Which ZKP system? (zk-STARKs for no trusted setup, or zk-SNARKs for smaller proofs?)
- How to handle biometric changes over time (aging, injury)?
- What is the false positive/negative rate for uniqueness verification?

### Success Criteria

- Identity system works with 1,000+ test identities
- ZKP verification time < 1 second on consumer hardware
- False positive rate for duplicate detection < 0.01%
- Security audit with no critical findings

---

## Phase 2: Single Node Prototype (L1)

**Duration**: 12–18 months
**Dependencies**: Phase 1 (identity system functional)

### Objectives

- [ ] Implement base node governance with sortition
- [ ] Implement VRF-based sortition selection
- [ ] Implement term tracking and cooldown enforcement
- [ ] Implement basic norm creation and voting
- [ ] Implement recall mechanism
- [ ] Deploy and test with 3–5 real communities (50–200 members each)

### Technical Milestones

1. **VRF implementation**: Implement ECVRF (RFC 9381) for sortition randomness
2. **Sortition engine**: Implement the full selection protocol from `sortition-protocol.md`
3. **Governance state machine**: Implement the node governance lifecycle (selection → service → audit → rotation)
4. **Norm engine (basic)**: Implement a simplified norm creation and voting system
5. **User interface**: Build a web/mobile interface for node participation

### Pilot Communities

Recruit 3–5 communities willing to use PolOS for internal governance:
- A housing cooperative
- A community garden or makerspace
- An online community (e.g., open-source project)
- A neighborhood association
- A worker cooperative

### Success Criteria

- At least 3 communities complete a full governance cycle (selection → service → rotation)
- Sortition selection is verifiable by all members
- No governance deadlocks or failures during pilot
- Member satisfaction survey: >70% positive

---

## Phase 3: Node Network (L2)

**Duration**: 18–24 months
**Dependencies**: Phase 2 (single node proven)

### Objectives

- [ ] Implement aggregation layer
- [ ] Implement inter-node sortition (stratified sampling)
- [ ] Implement subsidiarity enforcement
- [ ] Implement distributed VRF (threshold cryptography)
- [ ] Deploy network of 10–20 interconnected nodes
- [ ] Test aggregation-level governance

### Technical Milestones

1. **Distributed VRF**: Implement threshold VRF with Byzantine fault tolerance
2. **Aggregation protocol**: Implement node federation, membership, and exit
3. **Stratified sortition**: Implement proportional representation across nodes
4. **Subsidiarity engine**: Implement scope analysis and level assignment for decisions
5. **Inter-node communication**: Implement secure, authenticated communication between nodes

### Success Criteria

- Network of 10+ nodes with 1,000+ total members
- Aggregation-level governance decisions made and executed
- Subsidiarity correctly routes decisions to appropriate level
- Distributed VRF withstands simulated Byzantine attacks

---

## Phase 4: Norm Engine (L3)

**Duration**: 24–36 months
**Dependencies**: Phase 3 (network operational)

### Objectives

- [ ] Design the formal norm DSL
- [ ] Implement norm compiler (DSL → executable)
- [ ] Implement conflict detection
- [ ] Implement norm versioning and history
- [ ] Implement the 4-tier language model (L6)
- [ ] Deploy norm engine to the node network

### Technical Milestones

1. **DSL design**: Publish formal grammar and semantics for the norm language
2. **Norm compiler**: Implement compilation from DSL to deterministic executable form
3. **Conflict detector**: Implement static analysis for norm conflicts
4. **Language model**: Implement Tier 0 (formal) and Tier 1 (anchor translation) rendering
5. **Norm lifecycle**: Implement the full proposal → compilation → deployment → audit pipeline

### Research Questions to Resolve

- What is the right level of expressiveness for the DSL? (Too simple = can't express needed norms. Too complex = Gödelian concerns.)
- How to handle norm interactions that aren't direct conflicts but produce unintended emergent behavior?
- How to make the DSL accessible to non-programmers?

### Success Criteria

- DSL can express all norms currently in use by pilot communities
- Conflict detection catches known conflict patterns
- Tier 1 translations reviewed and approved by native speakers in 3+ languages
- Norm execution is deterministic across all validators

---

## Phase 5: Scale and Adoption

**Duration**: Ongoing
**Dependencies**: Phase 4 (norm engine operational)

### Objectives

- [ ] Implement automatic execution layer (L4)
- [ ] Implement continuous audit layer (L5)
- [ ] Scale to 100+ nodes, 10,000+ members
- [ ] Publish academic papers on results
- [ ] Engage with policy makers and governance researchers
- [ ] Explore legal frameworks for PolOS governance recognition

### Long-Term Vision

- PolOS as a viable alternative governance system operating alongside existing states
- Formal recognition of PolOS governance decisions by at least one jurisdiction
- Academic validation of sortition-based governance at scale
- Open-source ecosystem of tools, libraries, and applications built on PolOS

---

## Principles for Roadmap Execution

1. **Eat our own dog food**: The PolOS project itself should be governed by sortition as soon as Phase 0 is complete.
2. **Real communities, not simulations**: Every phase after Phase 0 involves real people making real decisions.
3. **Publish everything**: All research, code, data, and results are public domain.
4. **Honest failure reporting**: If something doesn't work, we publish that too.
5. **No rushing**: Governance is too important to ship broken. Each phase must meet its success criteria before the next begins.

## Timeline Summary

```
Year 1:     Phase 0 (Foundation) ──────────────────────►
Year 1-2:   Phase 1 (Identity) ────────────────────────────────►
Year 2-3:   Phase 2 (Single Node) ─────────────────────────────────────►
Year 3-4:   Phase 3 (Node Network) ────────────────────────────────────────────►
Year 4-6:   Phase 4 (Norm Engine) ─────────────────────────────────────────────────────►
Year 5+:    Phase 5 (Scale) ───────────────────────────────────────────────────────────────►
```

This is an ambitious timeline. It may take longer. That's fine. Getting it right matters more than getting it fast.
