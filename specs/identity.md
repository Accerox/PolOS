# L0: Sovereign Identity Specification

## 1. Overview

The Sovereign Identity system is Layer 0 of PolOS — the foundation upon which all other layers depend. It answers one question: **How do you prove you are a unique, real human being without surrendering your identity to a central authority?**

## 2. Requirements

### 2.1 Functional Requirements

| ID | Requirement | Priority |
|----|------------|----------|
| ID-F01 | One person = one identity (Sybil resistance) | Critical |
| ID-F02 | Identity is derived from biometric data | Critical |
| ID-F03 | Biometric data is never stored or transmitted | Critical |
| ID-F04 | Identity owner can prove membership in a set without revealing which member (ZKP) | Critical |
| ID-F05 | Identity can be revoked only by the owner | Critical |
| ID-F06 | Identity persists across nodes and aggregations | High |
| ID-F07 | Identity creation does not require internet connectivity (offline capable) | Medium |
| ID-F08 | Identity verification completes in < 1 second on consumer hardware | High |

### 2.2 Non-Functional Requirements

| ID | Requirement | Target |
|----|------------|--------|
| ID-N01 | False positive rate (two different people get same identity) | < 0.001% |
| ID-N02 | False negative rate (same person gets two identities) | < 0.01% |
| ID-N03 | ZKP proof size | < 1 KB |
| ID-N04 | ZKP verification time | < 500 ms |
| ID-N05 | Identity key derivation time | < 5 seconds |
| ID-N06 | System availability | 99.9% |

## 3. Identity Lifecycle

```
┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
│  Create   │───►│  Active   │───►│  Revoked  │───►│  Expired  │
│           │    │           │    │ (by owner)│    │           │
└──────────┘    └─────┬─────┘    └──────────┘    └──────────┘
                      │
                      ▼
                ┌──────────┐
                │  Renewed  │ (periodic re-verification)
                │           │
                └──────────┘
```

### 3.1 Creation

```
Input:  biometric_data (e.g., iris scan, fingerprint)
Output: (identity_key, identity_commitment, creation_proof)

Process:
1. Capture biometric data locally (on user's device)
2. Derive identity key:
   identity_key = KDF(biometric_hash(biometric_data), salt)
   where KDF is a key derivation function (e.g., Argon2id)
   and biometric_hash is a fuzzy hash tolerant of minor variations
3. Generate identity commitment:
   identity_commitment = Commit(identity_key)
   (a cryptographic commitment that hides the key but can be opened later)
4. Generate creation proof:
   creation_proof = ZKP{
     "I know a biometric input that produces this commitment,
      and this commitment is not in the existing commitment set"
   }
5. Publish identity_commitment to the identity registry
6. DELETE biometric_data from device
7. Store identity_key securely on user's device (encrypted)
```

### 3.2 Verification

```
Input:  identity_key, claim (e.g., "I am a member of Node X")
Output: (proof, verified: bool)

Process:
1. Generate ZKP:
   proof = ZKP{
     "I know an identity_key whose commitment is in the registry,
      AND that identity is a member of Node X,
      WITHOUT revealing which identity"
   }
2. Verifier checks:
   verified = Verify(proof, public_inputs: [registry_root, node_membership_root])
```

### 3.3 Revocation

```
Input:  identity_key, revocation_reason (optional)
Output: revocation_proof

Process:
1. Owner generates revocation proof:
   revocation_proof = Sign(identity_key, "REVOKE" || timestamp)
2. Publish revocation to registry
3. Identity commitment is moved to revocation set
4. All future verifications against this commitment will fail
5. Owner may create a new identity (new biometric capture required)
```

### 3.4 Renewal

Biometric data changes over time (aging, injury). Periodic renewal ensures continued access:

```
Input:  current_identity_key, new_biometric_data
Output: (new_identity_key, migration_proof)

Process:
1. Capture new biometric data
2. Derive new identity key from new biometric data
3. Generate migration proof:
   migration_proof = ZKP{
     "I know the old identity_key AND the new biometric input,
      and I am migrating from old_commitment to new_commitment"
   }
4. Update registry: replace old_commitment with new_commitment
5. DELETE new biometric data from device
```

## 4. Cryptographic Primitives

### 4.1 Biometric Hash

The biometric hash must be:
- **Fuzzy**: Tolerant of minor variations in biometric capture (lighting, angle, minor injuries)
- **Discriminative**: Different people produce different hashes with high probability
- **One-way**: Cannot reconstruct biometric data from the hash

**Candidate**: Locality-Sensitive Hashing (LSH) applied to biometric feature vectors, followed by a standard cryptographic hash.

### 4.2 Key Derivation Function

```
identity_key = Argon2id(
    password = biometric_hash(biometric_data),
    salt = device_salt || global_salt,
    time_cost = 3,
    memory_cost = 65536,  -- 64 MB
    parallelism = 4,
    output_length = 32    -- 256 bits
)
```

### 4.3 Commitment Scheme

```
identity_commitment = Pedersen_Commit(identity_key, randomness)

Properties:
- Hiding: commitment reveals nothing about identity_key
- Binding: cannot open commitment to a different key
- Homomorphic: supports ZKP operations
```

### 4.4 Zero-Knowledge Proof System

**Primary candidate**: zk-STARKs (Scalable Transparent Arguments of Knowledge)

**Rationale**:
- No trusted setup (transparent)
- Post-quantum secure
- Larger proof size than zk-SNARKs, but acceptable for identity proofs

**Alternative**: Groth16 (zk-SNARK) if proof size is critical and trusted setup is acceptable.

### 4.5 Identity Registry

The identity registry is a **Sparse Merkle Tree** (SMT):

```
Registry := SparseMerkleTree<identity_commitment → status>

status ∈ {active, revoked}

Operations:
- Insert(commitment): O(log n)
- Lookup(commitment): O(log n)
- Prove_membership(commitment): O(log n), proof size O(log n)
- Prove_non_membership(commitment): O(log n), proof size O(log n)
```

The registry root hash is published to a decentralized append-only log, ensuring no retroactive modification.

## 5. Sybil Resistance

### 5.1 The Core Challenge

Preventing one person from creating multiple identities without a central authority to check.

### 5.2 Multi-Layer Approach

```
Layer 1: Biometric uniqueness
    - Biometric hash collision resistance prevents same person from 
      generating two different-looking identities from the same biometrics
    - Fuzzy matching catches minor variations

Layer 2: Social verification
    - New identity creation requires attestation from k existing members
    - Attestors stake their reputation (false attestation = penalty)
    - Graph analysis detects suspicious attestation patterns

Layer 3: Behavioral analysis (optional, privacy-preserving)
    - Aggregate behavioral patterns (participation frequency, timing)
    - Anomaly detection for identities that behave like the same person
    - All analysis is on aggregate data, not individual behavior
```

### 5.3 Uniqueness Proof

```
At identity creation, the creator must prove:

ZKP{
    "My biometric_hash is not within distance d of any existing 
     biometric_hash in the registry"
}

This requires:
1. The registry stores hashed biometric commitments (not raw biometrics)
2. The ZKP circuit computes distance in the hash space
3. Distance threshold d is calibrated to biometric modality
```

## 6. Privacy Properties

| Property | Guarantee |
|----------|-----------|
| Identity unlinkability | Two verifications by the same person cannot be linked (different ZKPs each time) |
| Biometric privacy | Biometric data never leaves the user's device |
| Membership privacy | Can prove membership without revealing which member |
| Activity privacy | Participation in governance is visible, but not linkable to real-world identity |
| Revocation privacy | Revocation is visible (commitment moves to revocation set) but not linkable to the person |

## 7. Threat Model

| Threat | Mitigation |
|--------|-----------|
| Stolen device (identity key compromised) | Biometric re-derivation on new device; revoke old key |
| Coerced identity creation | Social verification layer; attestors can flag concerns |
| Biometric spoofing | Liveness detection at capture; multi-modal biometrics |
| Registry manipulation | Decentralized registry with append-only log; Merkle proofs |
| Quantum computing | zk-STARKs are post-quantum; identity keys can be migrated |
| Mass surveillance | ZKP prevents linking verifications; no central biometric database |

## 8. Open Questions

1. **Which biometric modality?** Iris scanning is most discriminative but requires specialized hardware. Fingerprint is ubiquitous but less discriminative. Face recognition has privacy concerns. Multi-modal (combining modalities) is most robust but most complex.

2. **Fuzzy hash calibration**: How much variation should the biometric hash tolerate? Too little = false negatives (same person can't re-derive key). Too much = false positives (different people get same hash).

3. **Device dependency**: The identity key is stored on the user's device. What happens if the device is lost and the user cannot re-derive from biometrics (e.g., due to injury)?

4. **Accessibility**: How do people with disabilities that affect biometric capture participate? (e.g., missing fingers, eye conditions)

5. **Children and minors**: At what age can a person create an identity? How are children represented in governance?

## 9. Relationship to Axioms

| Axiom | How L0 Implements It |
|-------|---------------------|
| Axiom 1 (Sovereign Identity) | Directly — this is the specification of Axiom 1 |
| Axiom 2 (Sortition Selection) | Provides the identity infrastructure for eligibility verification |
| Axiom 4 (Transparency) | Identity commitments are public; identity itself is private |
| Axiom 7 (Right of Exit) | Identity persists after leaving a node; no node controls identity |
