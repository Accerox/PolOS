# PolOS — Political Operating System

> *"Sortition is natural to democracy; election is natural to aristocracy."*
> — Montesquieu, *The Spirit of the Laws* (1748)

## What Is This?

PolOS is a formal specification for a **Political Operating System** — a decentralized, cryptographically verifiable governance framework built on one radical premise:

**True democracy requires sortition (random selection), not elections.**

Elections, by their nature, select for wealth, charisma, and connections — producing a permanent political class indistinguishable from aristocracy. Sortition — the method Athens used for 200 years — selects ordinary citizens, making governance genuinely representative.

PolOS combines this ancient insight with modern technology:

- **Cryptographic identity** — one person, one vote, zero-knowledge verified
- **Verifiable randomness** — sortition that no one can rig
- **Formal norm specification** — laws as deterministic, auditable code
- **Decentralized execution** — no single point of control or failure

This is not a blockchain project. This is not a voting app. This is a **complete operating system for governance** — from identity to norm execution to cultural translation.

## Core Thesis

Representative democracy has failed its promise. What we call "democracy" is actually **elective aristocracy**: a system where a self-perpetuating political caste governs in its own interest while performing periodic rituals of legitimation called "elections."

The solution is not better elections. The solution is **no elections at all** — replaced by:

1. **Sortition** — random selection of citizens for governance roles
2. **Rotation** — mandatory term limits with no consecutive re-selection
3. **Transparency** — all decisions cryptographically verified and publicly auditable
4. **Subsidiarity** — decisions made at the lowest possible level
5. **Right of exit** — unconditional freedom to leave any governance unit

## Architecture Overview

PolOS is structured in 7 layers, from identity to cultural interface:

```
┌─────────────────────────────────────────────┐
│  L6  Cultural Interface                     │  Multi-language rendering
├─────────────────────────────────────────────┤
│  L5  Continuous Audit                       │  AI-powered transparency
├─────────────────────────────────────────────┤
│  L4  Automatic Execution                    │  Smart contracts for governance
├─────────────────────────────────────────────┤
│  L3  Norm Engine                            │  Formal DSL, deterministic rules
├─────────────────────────────────────────────┤
│  L2  Aggregation                            │  Federation of nodes
├─────────────────────────────────────────────┤
│  L1  Base Node                              │  Minimum community unit
├─────────────────────────────────────────────┤
│  L0  Sovereign Identity                     │  Biometric-derived, self-sovereign
└─────────────────────────────────────────────┘
```

Each layer is formally specified with mathematical axioms that form an **immutable kernel** — rules that no process, majority, or entity can modify.

## Documentation

### Philosophy
- [`docs/philosophy.md`](docs/philosophy.md) — Why elections fail and sortition works

### Architecture
- [`docs/architecture.md`](docs/architecture.md) — Full 7-layer technical architecture
- [`docs/language-model.md`](docs/language-model.md) — 4-tier language rendering system

### Formal Specifications
- [`formal/axioms.md`](formal/axioms.md) — Mathematical axioms (immutable kernel)
- [`formal/sortition-protocol.md`](formal/sortition-protocol.md) — Sortition protocol specification

### Component Specifications
- [`specs/identity.md`](specs/identity.md) — L0 Sovereign Identity system
- [`specs/node.md`](specs/node.md) — L1 Base Node specification

### Project
- [`docs/roadmap.md`](docs/roadmap.md) — Development phases and milestones
- [`docs/open-problems.md`](docs/open-problems.md) — Unsolved problems for deliberation

## Source Documents

PolOS synthesizes two foundational documents:

1. **DPS Bootstrap v0.2** (*Distributed Political System*) — Technical architecture for a political operating system with formal axioms, a 7-layer stack, and a language-as-display-layer model.

2. **Manifiesto de la Democracia Real** — Political philosophy manifesto arguing that representative democracy is elective aristocracy, and that sortition is the only form of genuine democracy, with historical evidence from Athens (507 BC–322 BC).

## Contributing

This project is in its **foundational phase** (Phase 0). Contributions are welcome in:

1. **Formal specification review** — Are the axioms consistent? Complete? Are there edge cases?
2. **Protocol design** — How should sortition work at scale? What VRF scheme is best?
3. **Identity research** — Sybil-resistant identity without centralized biometrics?
4. **Philosophy and history** — Additional evidence for or against sortition?
5. **Translation** — The language model needs anchor translations in every language.

### How to Contribute

1. Read the relevant specification documents
2. Open an issue describing your proposed change or addition
3. Reference specific axioms, layers, or open problems
4. Submit a pull request with your changes

### Code of Conduct

This project discusses governance — a topic that invites strong opinions. We ask that contributors:

- Argue with evidence, not ideology
- Engage with the formal specifications, not just the philosophy
- Acknowledge unsolved problems honestly
- Respect that reasonable people can disagree on governance

## Status

**Phase 0: Foundation** — Formal specifications and community building.

No code has been written yet. PolOS is currently a set of formal specifications and philosophical arguments. The first implementation milestone is a sovereign identity prototype (Phase 1).

See [`docs/roadmap.md`](docs/roadmap.md) for the full development plan.

## License

**Public Domain** — This work is dedicated to the public domain under [CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/).

Governance belongs to everyone. No one should own the specification for how humans organize themselves.

## Etymology

**PolOS** = **Pol**itical **O**perating **S**ystem

Also evocative of *polis* (πόλις) — the ancient Greek city-state where democracy was born, and where sortition was the standard method of selecting officials for two centuries.
