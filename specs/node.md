# L1: Base Node Specification

## 1. Overview

A **Base Node** is the minimum unit of self-governance in PolOS. It is a community of people who have chosen to govern themselves using sortition-based democracy. Nodes are the building blocks of the entire system — all higher-level governance (aggregations, L2+) is built from federations of nodes.

## 2. Requirements

### 2.1 Functional Requirements

| ID | Requirement | Priority |
|----|------------|----------|
| ND-F01 | Nodes have a minimum size of 30 members | Critical |
| ND-F02 | Nodes have a maximum size of 500 members | High |
| ND-F03 | All governance roles are filled by sortition | Critical |
| ND-F04 | Members can join and leave freely (Axiom 7) | Critical |
| ND-F05 | Nodes can create, modify, and repeal norms (subject to axioms) | Critical |
| ND-F06 | Nodes can federate into aggregations (L2) | High |
| ND-F07 | Nodes maintain an immutable audit log of all governance actions | Critical |
| ND-F08 | Nodes can split when they exceed maximum size | Medium |
| ND-F09 | Nodes can merge when they fall below minimum size | Medium |

### 2.2 Non-Functional Requirements

| ID | Requirement | Target |
|----|------------|--------|
| ND-N01 | Governance action latency (proposal to decision) | < 7 days for standard norms |
| ND-N02 | Sortition selection time | < 1 minute |
| ND-N03 | Audit log query time | < 1 second |
| ND-N04 | System availability | 99.9% |

## 3. Node Lifecycle

```
┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
│  Founded  │───►│  Active   │───►│  Merging  │───►│ Dissolved │
│           │    │           │    │           │    │           │
└──────────┘    └─────┬─────┘    └──────────┘    └──────────┘
                      │
                      ├───► Splitting ───► 2 Active Nodes
                      │
                      └───► Federating ───► Member of Aggregation
```

### 3.1 Founding

A node is founded when:

```
1. A group of ≥ 30 people with verified identities (L0) declare intent 
   to form a node.
2. The founding group ratifies the PolOS axioms (non-negotiable).
3. The founding group adopts an initial norm set:
   - Minimum: governance structure (council size, term length, meeting frequency)
   - The initial norms are proposed by the founders and ratified by 
     supermajority (2/3) of the founding group.
4. The first sortition is conducted to select the initial governance body.
5. The node is registered in the PolOS network.
```

**Bootstrap note**: The founding group acts as a temporary governance body until the first sortition. This is the only non-sortition governance in the node's history.

### 3.2 Membership

```
Joining:
1. Person presents verified identity (L0)
2. Person reviews and accepts the node's current norms
3. Person is added to the membership registry
4. Person becomes eligible for sortition after a probation period 
   (default: 30 days, configurable by node norm)

Leaving:
1. Person declares intent to leave (no reason required — Axiom 7)
2. If currently serving in a governance role:
   - Role is transferred to the next alternate
   - If no alternate, emergency sortition is triggered
3. Person is removed from membership registry
4. Person's identity persists (L0) — they can join another node
5. Pre-existing obligations remain (e.g., commitments made during service)
```

### 3.3 Size Management

**Splitting** (when membership exceeds 500):

```
1. Node governance body proposes a split.
2. Members choose which resulting node to join (or leave entirely).
3. If both resulting nodes have ≥ 30 members, the split proceeds.
4. Each resulting node conducts fresh sortition for governance.
5. Shared resources are divided by agreement or, failing that, by 
   sortition-selected arbitration panel.
```

**Merging** (when membership falls below 30):

```
1. Node governance body proposes merger with a neighboring node.
2. Both nodes vote on the merger (simple majority in each).
3. If approved, memberships are combined.
4. If combined membership exceeds 500, the merged node must split.
5. Fresh sortition for the merged node's governance.
```

## 4. Governance Structure

### 4.1 Governance Body: The Council

Every node has a **Council** — the primary governance body, selected entirely by sortition.

```
Council composition:
- Size: max(7, ⌈members / 30⌉), capped at 15
  (e.g., 30 members → 7, 150 members → 7, 300 members → 10, 500 members → 15)
- Selection: Sortition from all eligible members
- Term: 6 months (configurable by node norm, max 1 year)
- Rotation: Staggered — half replaced every 3 months
- Cooldown: 1 full term after service
```

### 4.2 Specialized Roles

Within the council, specialized roles are assigned by sortition among council members:

| Role | Responsibilities | Term |
|------|-----------------|------|
| **Facilitator** | Chairs meetings, sets agenda, ensures process | 3 months |
| **Secretary** | Records decisions, maintains audit log | 3 months |
| **Treasurer** | Manages node finances (if any) | 6 months |
| **Auditor** | Reviews governance actions for axiom compliance | 3 months |
| **External liaison** | Represents node in aggregation governance | 6 months |

Roles rotate among council members. No council member holds the same role for consecutive terms.

### 4.3 Full Assembly

The **Full Assembly** is all members of the node. It convenes for:

- Ratification of new norms (proposed by the council)
- Recall votes
- Node split or merge decisions
- Any matter the council refers to the full assembly

```
Full Assembly rules:
- Quorum: 50% of members
- Standard decisions: Simple majority
- Norm changes: 2/3 supermajority
- Axiom-level decisions: Not possible (Axiom 6)
- Recall: 2/3 supermajority (see sortition-protocol.md §7)
```

## 5. Decision-Making Process

### 5.1 Standard Decision Flow

```
┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
│ Proposal  │───►│Delibera- │───►│  Council  │───►│ Execution │───►│  Audit   │
│           │    │  tion    │    │   Vote    │    │           │    │          │
└──────────┘    └──────────┘    └──────────┘    └──────────┘    └──────────┘
```

1. **Proposal**: Any member can submit a proposal. Proposals are published to all members.
2. **Deliberation**: The council discusses the proposal. Members can submit comments and questions. Minimum deliberation period: 48 hours.
3. **Council vote**: The council votes. Simple majority for standard decisions.
4. **Execution**: If approved, the decision is executed (manually or automatically via L4).
5. **Audit**: The decision is recorded in the immutable audit log. The auditor reviews for axiom compliance.

### 5.2 Norm Change Flow

Norm changes follow a more rigorous process:

```
┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
│ Proposal  │───►│  Formal   │───►│ Conflict  │───►│Delibera- │───►│ Assembly  │───►│  Deploy  │
│           │    │  Spec     │    │  Check    │    │  tion    │    │   Vote    │    │          │
└──────────┘    └──────────┘    └──────────┘    └──────────┘    └──────────┘    └──────────┘
```

1. **Proposal**: Any member proposes a norm change.
2. **Formal specification**: The proposal is translated into the formal DSL (L3). AI assistance available.
3. **Conflict check**: The norm engine checks for conflicts with existing norms and axioms.
4. **Deliberation**: Extended deliberation period (minimum 7 days). Expert input invited.
5. **Assembly vote**: The full assembly votes. 2/3 supermajority required.
6. **Deployment**: The norm is compiled and deployed to the execution layer (L4).

### 5.3 Emergency Decisions

For time-sensitive matters (e.g., security threats, infrastructure failures):

```
1. Any council member can declare an emergency.
2. Emergency requires confirmation by 2/3 of available council members.
3. Emergency decisions take effect immediately.
4. Emergency decisions automatically expire after 72 hours.
5. The full assembly must ratify or reject the emergency decision within 7 days.
6. If not ratified, the decision is reversed and the council must address 
   the situation through normal process.
```

## 6. Norm System

### 6.1 Norm Hierarchy

```
Level 0: Axioms (immutable, system-wide)
    ↓ constrains
Level 1: Node constitution (foundational norms, 2/3 supermajority to change)
    ↓ constrains
Level 2: Standard norms (regular governance rules, simple majority to change)
    ↓ constrains
Level 3: Operational decisions (day-to-day, council authority)
```

### 6.2 Norm Properties

Every norm must specify:

```
Norm := {
    id: unique identifier,
    title: human-readable name,
    formal_spec: DSL expression (L3),
    scope: what the norm governs,
    authority: who can invoke the norm,
    effective_date: when the norm takes effect,
    sunset_date: when the norm expires (optional),
    version: version number,
    history: list of all previous versions,
    vote_record: how the norm was approved
}
```

### 6.3 Norm Constraints

All norms must satisfy:

```
∀ norm ∈ NodeNorms:
    ¬violates(norm, Axioms)                    -- cannot violate axioms
    ∧ ¬conflicts(norm, higher_level_norms)     -- cannot conflict with higher norms
    ∧ scope(norm) ⊆ jurisdiction(node)         -- cannot exceed node's jurisdiction
    ∧ approved_by(norm, required_authority)     -- must be properly approved
```

## 7. Relationship with Other Nodes

### 7.1 Inter-Node Relations

Nodes are sovereign but can:

- **Federate**: Join an aggregation (L2) for inter-node governance
- **Cooperate**: Bilateral agreements with other nodes (trade, mutual aid, shared resources)
- **Communicate**: Exchange information, best practices, norm templates

### 7.2 Dispute Resolution

Disputes between nodes are resolved by:

```
1. Direct negotiation between node councils
2. If unresolved: mediation by a sortition-selected panel from both nodes
3. If unresolved: arbitration by the aggregation (if both nodes are members)
4. If no shared aggregation: mutual agreement on an ad-hoc arbitration panel
5. Ultimate fallback: nodes go their separate ways (Axiom 7)
```

## 8. Data Model

### 8.1 Node State

```
NodeState := {
    id: NodeID,
    name: String,
    members: Set<IdentityCommitment>,
    council: Set<IdentityCommitment>,
    roles: Map<GovernanceRole, IdentityCommitment>,
    norms: Set<Norm>,
    audit_log: AppendOnlyLog<GovernanceEvent>,
    aggregations: Set<AggregationID>,
    created_at: Timestamp,
    state_root: MerkleRoot  -- hash of entire state for verification
}
```

### 8.2 Governance Events

```
GovernanceEvent := {
    id: EventID,
    type: {proposal | vote | decision | norm_change | membership_change | 
           sortition | recall | emergency | split | merge},
    timestamp: Timestamp,
    actor: IdentityCommitment (or "system" for automated events),
    details: EventSpecificData,
    proof: CryptographicProof,
    previous_event: EventID  -- chain of events
}
```

## 9. Example: A Day in the Life of a Node

```
Morning:
- The node's 150 members wake up. 9 of them are on the council (selected 
  by sortition 2 months ago).
- The facilitator (a council member, selected by sortition among council 
  members last month) publishes today's agenda.

Midday:
- A member proposes a new norm: "All node meetings must be recorded and 
  published within 24 hours."
- The proposal is published to all members. Deliberation begins.
- The norm engine (L3) translates the proposal into formal DSL and checks 
  for conflicts. None found.

Afternoon:
- The council discusses three pending proposals. Two are approved by 
  majority vote. One is sent back for revision.
- The auditor reviews yesterday's decisions. All comply with axioms.

Evening:
- A member announces they are leaving the node (Axiom 7). No reason 
  required. Their membership is updated. They were not on the council, 
  so no governance impact.
- The treasurer publishes the monthly financial report. All members can 
  verify it against the audit log.

Next week:
- The deliberation period for the recording norm expires. The full 
  assembly votes. 112 of 149 members participate. 98 vote yes (87.5%). 
  The norm passes (exceeds 2/3 threshold).
- The norm is compiled and deployed. From now on, the system automatically 
  flags any meeting that hasn't been published within 24 hours.
```

## 10. Relationship to Axioms

| Axiom | How L1 Implements It |
|-------|---------------------|
| Axiom 1 (Sovereign Identity) | Membership requires verified L0 identity |
| Axiom 2 (Sortition Selection) | Council and all roles selected by sortition |
| Axiom 3 (No Consecutive Re-selection) | Cooldown periods enforced for all roles |
| Axiom 4 (Transparency) | Immutable audit log; all decisions public |
| Axiom 5 (Subsidiarity) | Node handles all matters within its jurisdiction |
| Axiom 6 (Immutability of Axioms) | Norms cannot violate axioms; conflict check enforced |
| Axiom 7 (Right of Exit) | Members can leave freely; no penalty |
| Axiom 8 (Norm Determinism) | Norms compiled to deterministic form via L3 |
