# Sortition Protocol Specification

## 1. Overview

This document specifies the technical protocol for sortition (random selection) in PolOS. Sortition is the mechanism by which all governance roles are filled — it is the core democratic primitive of the system.

The protocol must satisfy:
- **Unpredictability**: No party can predict the outcome before it occurs
- **Verifiability**: Any observer can verify the selection was genuinely random
- **Non-manipulability**: No single party (or coalition below threshold) can influence the result
- **Privacy**: The selected person's identity is not revealed until they choose to accept
- **Fairness**: Every eligible person has an equal probability of selection (weighted only by legitimate criteria)

## 2. Cryptographic Foundation: Verifiable Random Functions (VRFs)

### 2.1 VRF Definition

A Verifiable Random Function is a triple of algorithms `(KeyGen, Evaluate, Verify)`:

```
KeyGen() → (sk, pk)
    Generates a secret key sk and public key pk.

Evaluate(sk, input) → (output, proof)
    Produces a pseudorandom output and a proof of correctness.

Verify(pk, input, output, proof) → {true, false}
    Anyone can verify the output was correctly computed from the input
    using the secret key corresponding to pk, without knowing sk.
```

### 2.2 Proposed VRF Scheme

PolOS uses **ECVRF** as specified in [RFC 9381](https://www.rfc-editor.org/rfc/rfc9381), based on elliptic curve cryptography. Specifically:

- **Curve**: Ed25519 (Curve25519 in Edwards form)
- **Hash**: SHA-512
- **Suite**: ECVRF-EDWARDS25519-SHA512-ELL2

### 2.3 Distributed Randomness

No single party holds the VRF secret key. Instead, randomness is generated through a **distributed VRF (DVRF)** protocol:

```
1. Committee of k randomness providers is selected (itself by sortition 
   from the previous period).
2. Each provider i generates a partial evaluation:
   (output_i, proof_i) = Evaluate(sk_i, epoch || role || node_id)
3. Partial outputs are combined using threshold cryptography:
   final_output = Combine(output_1, ..., output_k)  [requires t-of-k]
4. Anyone can verify:
   Verify_distributed(pk_1, ..., pk_k, input, final_output, proofs) → true
```

**Threshold**: t = ⌈2k/3⌉ + 1 (Byzantine fault tolerant)

This ensures that:
- No single provider can predict or manipulate the output
- Up to ⌊(k-1)/3⌋ malicious providers cannot affect the result
- The output is deterministic given the honest providers' inputs

## 3. Selection Protocol

### 3.1 Inputs

```
SelectionInput := {
    epoch: T,                    -- current time period
    node_or_agg: N ∪ A,         -- the governance unit
    role: GovernanceRole,        -- the role to fill
    eligible_set: Set<Identity>, -- all eligible identities (see §4)
    num_to_select: ℕ,           -- how many to select
    randomness: VRF_Output       -- from the distributed VRF (§2.3)
}
```

### 3.2 Algorithm

```
function SortitionSelect(input: SelectionInput) → Set<Identity>:
    -- Step 1: Deterministic ordering
    ordered_list = sort(input.eligible_set, by = hash(identity || input.randomness))
    
    -- Step 2: Selection
    selected = ordered_list[0 : input.num_to_select]
    
    -- Step 3: Alternates
    alternates = ordered_list[input.num_to_select : input.num_to_select * 2]
    
    -- Step 4: Proof generation
    proof = generate_proof(input, selected, alternates)
    
    return (selected, alternates, proof)
```

**Properties**:
- The ordering is deterministic given the randomness — anyone can reproduce it
- The randomness is unpredictable before the VRF output is revealed
- Alternates are pre-selected to handle declines without a new sortition round

### 3.3 Acceptance Protocol

```
1. Selected identity is notified (via encrypted channel).
2. The person has acceptance_window time to respond.
3. If accepted:
   - Person reveals their identity to the node/aggregation
   - ZKP proves they are the selected identity
   - Term begins
4. If declined or no response:
   - Next alternate is notified
   - Process repeats until the role is filled
5. If all alternates exhausted:
   - New sortition round with remaining eligible set
```

## 4. Eligibility Criteria

### 4.1 Base Eligibility

A person `p` is eligible for role `r` in governance unit `g` at time `t` if and only if:

```
eligible(p, r, g, t) ⟺
    member(p, g, t)                           -- is a member of the unit
    ∧ V(identity(p))                          -- has a verified identity
    ∧ ¬serving(p, r, g, t)                    -- not currently serving in this role
    ∧ ¬in_cooldown(p, r, g, t)               -- not in cooldown period (Axiom 3)
    ∧ ¬opted_out(p, r, g, t)                 -- has not opted out (see §4.3)
    ∧ meets_requirements(p, r)                -- meets role-specific requirements
```

### 4.2 Role-Specific Requirements

Most roles have no requirements beyond base eligibility. Some roles may have minimal requirements:

| Role | Additional Requirements |
|------|----------------------|
| Council member | None |
| Facilitator | None |
| Treasurer | Basic numeracy certification (self-assessed, honor system) |
| Auditor | None (training provided after selection) |
| Norm reviewer | None (training provided after selection) |
| Execution validator | Technical capability to run validation software |

**Principle**: Requirements are kept minimal to avoid recreating a competence-based filter that would undermine sortition's representative nature. Training is provided after selection, not required before.

### 4.3 Opt-Out

Any person may opt out of sortition for a specific role or all roles:

```
opt_out(p, r, g) → p ∉ EligibilitySet(g, r, t) for all future t
    until opt_in(p, r, g)
```

Opt-out is unconditional and carries no penalty. However:
- Opt-out rates are publicly tracked (aggregate, not individual)
- If opt-out exceeds a threshold (e.g., 70%), the node must investigate and address the underlying cause

## 5. Term Limits and Rotation

### 5.1 Term Duration

| Governance Level | Default Term | Maximum Term | Cooldown |
|-----------------|-------------|-------------|----------|
| Node council | 6 months | 1 year | 1 full term |
| Node specific roles | 3 months | 6 months | 1 full term |
| Aggregation L1 council | 1 year | 2 years | 1 full term |
| Aggregation L2+ council | 1 year | 2 years | 2 full terms |
| Audit panel | 3 months | 6 months | 2 full terms |
| Norm review committee | 6 months | 1 year | 1 full term |

### 5.2 Staggered Rotation

To ensure continuity, governance bodies use staggered rotation:

```
For a council of size n:
    - n/2 members are replaced every term/2
    - Each replacement is a fresh sortition
    - This ensures at least n/2 experienced members at all times
```

### 5.3 Lifetime Limits

```
∀ p ∈ P, ∀ role ∈ GovernanceRoles:
    total_terms_served(p, role) ≤ lifetime_limit(role)

Default lifetime_limit = 3 terms per role per governance unit
```

This prevents any person from accumulating excessive governance experience in a single role, even with cooldown periods.

## 6. Emergency Provisions

### 6.1 Emergency Sortition

If a governance body loses quorum (e.g., due to mass resignation, natural disaster):

```
1. Emergency sortition is triggered automatically when:
   active_members(body) < quorum_threshold(body)
   
2. Emergency sortition uses:
   - Reduced acceptance_window (24 hours instead of 7 days)
   - Expanded eligible set (cooldown periods temporarily waived)
   - Simplified protocol (single VRF provider if distributed VRF unavailable)
   
3. Emergency terms are shorter (1/2 normal term)
   
4. Normal sortition resumes at next regular cycle
```

### 6.2 Continuity of Operations

If the sortition infrastructure itself is compromised:

```
1. The last validly-selected body continues to serve until infrastructure 
   is restored.
2. Their term is automatically extended, but capped at 2x normal term.
3. If infrastructure is not restored within 2x normal term:
   - The body must call a community assembly (all members)
   - The assembly selects a temporary body by physical lot (paper-based)
   - This temporary body serves until digital sortition is restored
```

## 7. Accountability: Recall Mechanism

Sortition does not mean unaccountability. Any sortition-selected official can be recalled:

### 7.1 Recall Process

```
1. Recall petition: Any member can initiate. Requires signatures from 
   10% of the governance unit's members.
   
2. Recall deliberation: A sortition-selected panel (not including the 
   person being recalled) reviews the petition and hears from both sides.
   
3. Recall vote: The full governance unit votes.
   - Threshold: 2/3 supermajority required for recall
   - Quorum: 50% of members must participate
   
4. If recalled:
   - The person is immediately removed from the role
   - An alternate (from the original sortition) fills the remainder of the term
   - The recalled person enters cooldown as if they completed a full term
   
5. If not recalled:
   - No further recall petition for the same person for the same conduct 
     can be filed for 3 months
```

### 7.2 Grounds for Recall

The recall mechanism is deliberately broad — the community decides what constitutes grounds for recall. However, the following are explicitly recognized:

- Failure to fulfill role duties
- Violation of the axioms
- Conflict of interest (undisclosed)
- Abuse of position

The recall mechanism is the **only** exception to the no-election principle. It is a safety valve, not a governance mechanism.

## 8. Scaling Sortition

### 8.1 Node Level (30–500 members)

At node level, sortition is straightforward:
- Eligible set is small enough to enumerate
- VRF output directly determines selection
- All members know each other (social accountability)

### 8.2 Aggregation Level (500–50,000 members)

At aggregation level:
- Eligible set is the union of all member nodes' eligible sets
- **Stratified sampling**: Ensure each node is represented proportionally
- Selection is still direct (from the full eligible set), but with diversity constraints

```
For an aggregation of nodes N_1, ..., N_m selecting a council of size c:
    min_per_node = max(1, ⌊c × |N_i| / Σ|N_j|⌋)
    -- Each node gets at least proportional representation
    -- Remaining seats filled by unrestricted sortition from full eligible set
```

### 8.3 Large Aggregation Level (50,000–1,000,000+ members)

At large scale:
- **Two-stage sortition**: First, select a large pool (e.g., 10x council size). Then, from that pool, select the final council.
- **Geographic stratification**: Ensure geographic diversity
- **Demographic monitoring**: Track (aggregate, not individual) whether the selected body is statistically representative. If systematic bias is detected, investigate the eligibility criteria.

### 8.4 Statistical Properties

For a population of size `n` selecting a body of size `c`:

```
Expected representation of any subgroup of proportion p:
    E[selected from subgroup] = c × p
    
Standard deviation:
    σ = √(c × p × (1-p))
    
95% confidence interval:
    c × p ± 1.96 × √(c × p × (1-p))
```

Example: In a population that is 51% women, a council of 100 will have between 41 and 61 women with 95% confidence. A council of 500 will have between 232 and 278 women with 95% confidence.

Larger councils are more representative. This is a design consideration when setting council sizes.

## 9. Protocol Security Analysis

### 9.1 Threat Model

| Threat | Mitigation |
|--------|-----------|
| VRF key compromise | Distributed VRF with threshold t-of-k |
| Sybil attack (fake identities) | L0 biometric-derived identity with ZKP |
| Collusion among selected | Short terms, rotation, audit (L5) |
| Selective non-participation | Alternates, emergency provisions |
| Eligibility manipulation | Eligibility computed from immutable state |
| Timing attack (predict VRF input) | Epoch and role are public; randomness comes from VRF |
| Denial of service | Decentralized infrastructure, fallback to physical lot |

### 9.2 Formal Security Properties

```
Unpredictability:
    Pr[adversary predicts selection | controls < t providers] < negl(λ)

Non-manipulability:
    Pr[adversary influences selection | controls < t providers] < negl(λ)

Verifiability:
    ∀ selection: ∃ proof such that Verify(proof) = true ∧ 
    proof is publicly checkable

Fairness:
    ∀ p_1, p_2 ∈ EligibilitySet:
    Pr[selected(p_1)] = Pr[selected(p_2)] = |council| / |EligibilitySet|
```

Where `negl(λ)` is a negligible function of the security parameter `λ`.

## 10. Implementation Roadmap

1. **Phase 1**: Single-node sortition with centralized VRF (prototype)
2. **Phase 2**: Distributed VRF with threshold cryptography
3. **Phase 3**: Multi-node sortition with stratified sampling
4. **Phase 4**: Large-scale sortition with two-stage selection
5. **Phase 5**: Formal verification of protocol properties
