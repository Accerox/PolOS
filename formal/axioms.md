# Formal Axiom Specification

## Preamble

These axioms form the **immutable kernel** of PolOS. They are structural constraints on governance — the minimum conditions under which a system can be called democratic.

No process, majority, entity, or aggregation of entities can modify these axioms (Axiom 6). They are not laws — they are the rules that constrain what laws can be.

## Primitive Types

```
P := set of all persons (physical human beings)
N := set of all nodes (base communities, L1)
A := set of all aggregations (federations of nodes, L2+)
R := set of all rules (norms expressed in the formal DSL, L3)
D := set of all decisions (outputs of governance processes)
I := set of all identities (sovereign, biometric-derived, L0)
T := time domain (discrete, append-only, monotonically increasing)
V := verified(x) — a cryptographically proven proposition about x
```

## Derived Types

```
GovernanceRoles := {council_member, facilitator, treasurer, auditor, 
                    norm_reviewer, conflict_resolver, audit_panelist, 
                    execution_validator, translation_reviewer, ...}

Term := (role: GovernanceRole, holder: P, start: T, end: T, node_or_agg: N ∪ A)

EligibilitySet(N, role, t) := {p ∈ members(N) : eligible(p, role, t)}

R_axiom ⊂ R := the set of axioms (this document)
R_norm ⊂ R := the set of enacted norms (modifiable through governance)
R = R_axiom ∪ R_norm, R_axiom ∩ R_norm = ∅
```

## Core Axioms

### Axiom 1: Sovereign Identity

```
∀ p ∈ P: ∃! i ∈ I such that:
    owns(p, i)
    ∧ ¬transferable(i)
    ∧ ¬revocable_by_others(i)
    ∧ biometric_derived(i)
    ∧ V(uniqueness(i))
```

**In plain language**: Every person has exactly one identity. That identity is non-transferable, cannot be revoked by anyone other than the person themselves, is derived from biometric data (not storing it), and its uniqueness is cryptographically verified.

**Implications**:
- No person can have multiple identities (Sybil resistance)
- No authority can strip a person of their identity
- Identity verification does not require revealing identity (ZKP)

### Axiom 2: Sortition Selection

```
∀ N ∈ (Nodes ∪ Aggregations), ∀ role ∈ GovernanceRoles:
    selection(role, N) = verifiable_random_sample(EligibilitySet(N, role, t_current))
    ∧ V(randomness_source(selection))
    ∧ term(role) ≤ max_term(role)
    ∧ |sample| = required_size(role, N)
```

**In plain language**: All governance roles in all nodes and aggregations are filled by verifiable random selection from the set of eligible members. The randomness is cryptographically proven genuine. Terms have maximum durations. The sample size matches the role's requirements.

**Implications**:
- No elections for any governance role
- The randomness must be verifiable by any observer
- Term limits are enforced structurally, not by convention

### Axiom 3: No Consecutive Re-selection

```
∀ p ∈ P, ∀ role ∈ GovernanceRoles, ∀ N ∈ (Nodes ∪ Aggregations):
    served(p, role, N, term_t) → p ∉ EligibilitySet(N, role, t) 
    for all t ∈ [end(term_t), end(term_t) + cooldown(role)]
```

**In plain language**: After serving in a governance role, a person is ineligible for the same role in the same node/aggregation for a cooldown period. This prevents the formation of a permanent governing class.

**Implications**:
- No political careers
- Governance experience is distributed across the population
- The cooldown period is role-specific (configurable per norm, but must be ≥ 1 full term)

### Axiom 4: Transparency

```
∀ d ∈ D:
    public(d)
    ∧ V(d)
    ∧ auditable(d)
    ∧ ∃ proof ∈ AuditLog: verifies(proof, d)
    ∧ timestamp(d) ∈ T
    ∧ immutable_after(d, timestamp(d))
```

**In plain language**: Every governance decision is public, cryptographically verified, auditable, recorded in an immutable log with a timestamp, and cannot be altered after the fact.

**Implications**:
- No secret governance
- All decisions have cryptographic proofs of correctness
- The audit log is append-only — history cannot be rewritten

**Note**: Transparency applies to governance actions, not to individual identity. A person's vote in a sortition-selected council may be public, but their identity is protected by ZKP at L0.

### Axiom 5: Subsidiarity

```
∀ d ∈ D:
    level(d) = min{l ∈ Levels : scope(d) ⊆ jurisdiction(l)}

where:
    Levels = {node, aggregation_1, aggregation_2, ...}
    jurisdiction(node) = members(node)
    jurisdiction(agg) = ∪{members(n) : n ∈ children(agg)}
```

**In plain language**: Every decision is made at the lowest governance level whose jurisdiction covers the decision's scope. A decision that affects only one node is made by that node. A decision that affects multiple nodes is made by their aggregation — but only if it genuinely requires inter-node coordination.

**Implications**:
- Power defaults to the local level
- Higher levels must justify their jurisdiction
- Scope creep is structurally prevented

### Axiom 6: Immutability of Axioms

```
∀ r ∈ R_axiom:
    ¬∃ process(π) such that:
        execute(π) → modify(r) ∨ repeal(r) ∨ suspend(r)

    This holds regardless of:
        - majority size (even unanimity cannot modify axioms)
        - governance level (no aggregation can override axioms)
        - emergency status (no exception for crisis)
```

**In plain language**: No process — no vote, no consensus, no emergency decree, no constitutional amendment — can modify, repeal, or suspend the axioms. They are hardcoded into the system.

**Implications**:
- The axioms are the system's DNA — they define what PolOS is
- This is the most controversial axiom — see [`open-problems.md`](../docs/open-problems.md) for discussion
- The axioms constrain what norms can exist, but do not dictate specific policies

**Justification**: If the axioms could be modified by majority vote, then a sufficiently large majority could abolish sortition and reinstate elections — destroying the system from within. Immutability prevents this. The axioms are deliberately minimal — they constrain structure, not policy.

### Axiom 7: Right of Exit

```
∀ p ∈ P, ∀ N ∈ (Nodes ∪ Aggregations):
    can_leave(p, N) unconditionally
    ∧ ¬∃ penalty(leaving(p, N))
    ∧ identity(p) persists after leaving(p, N)
    ∧ obligations(p, N) terminate upon leaving(p, N)
        except: obligations_incurred_before(leaving(p, N))
```

**In plain language**: Any person can leave any node or aggregation at any time, without penalty. Their identity persists. Their future obligations to the node cease. Pre-existing obligations (e.g., debts) remain.

**Implications**:
- Governance is consensual — no one is trapped
- Nodes must earn their members' continued participation
- This is the ultimate check on bad governance: people leave

### Axiom 8: Norm Determinism

```
∀ r ∈ R, ∀ input ∈ ValidInputs(r):
    execute(r, input) is deterministic
    ∧ V(execute(r, input))
    ∧ execute(r, input) = execute(r, input)  [idempotent for same state]
    ∧ ∀ validator_1, validator_2:
        execute_by(validator_1, r, input) = execute_by(validator_2, r, input)
```

**In plain language**: Given the same inputs and state, a norm always produces the same output. The execution is cryptographically verifiable. Any two validators executing the same norm on the same input will get the same result.

**Implications**:
- No room for "interpretation" — norms are unambiguous
- Execution can be independently verified by anyone
- Disputes about norm application are resolved by re-execution, not by authority

## Meta-Properties

The axiom system as a whole satisfies:

### Consistency

```
¬∃ (r_1, r_2) ∈ R_axiom × R_axiom such that:
    r_1 ∧ r_2 → ⊥  (contradiction)
```

The axioms do not contradict each other.

### Independence

```
∀ r ∈ R_axiom:
    R_axiom \ {r} ⊬ r
```

No axiom can be derived from the others. Each axiom adds a genuinely new constraint.

### Minimality

The axiom set is minimal — no axiom can be removed without losing a structural guarantee that the system requires.

## Open Questions

1. **Is Axiom 6 too strong?** Can a system truly be immutable? What if a fundamental flaw is discovered in an axiom? See [`open-problems.md`](../docs/open-problems.md).

2. **Is the axiom set complete?** Are there structural guarantees that democracy requires which are not captured by these 8 axioms?

3. **Formal verification**: Can the consistency and independence of the axioms be machine-verified? This is a goal for Phase 1.

4. **Gödel considerations**: Any sufficiently powerful formal system is either incomplete or inconsistent (Gödel's incompleteness theorems). The axiom system is deliberately kept simple to avoid this — it constrains structure, not computation. But the interaction with the Norm Engine (L3) may introduce Gödelian concerns.
