# Open Problems for Deliberation

## Preamble

PolOS is an ambitious project. Intellectual honesty requires acknowledging the problems we have not solved. This document catalogs the open problems — not to discourage, but to focus research and deliberation.

Each problem includes:
- **Description**: What the problem is
- **Why it matters**: Why it can't be ignored
- **Current thinking**: Preliminary approaches (if any)
- **Related axioms**: Which axioms are affected

---

## 1. The Bootstrap Problem

### Description

Who writes the initial rules? The system requires norms to function, but norms are created through governance processes that don't exist yet. The first node cannot use sortition to select its governance body because the sortition protocol hasn't been ratified yet.

### Why It Matters

Every governance system faces this problem, but PolOS faces it acutely because it rejects the legitimacy of any non-sortition governance process. If the founders write the initial rules, they are an unelected elite — exactly what the system is designed to prevent.

### Current Thinking

- **Constitutional convention approach**: A group of founders writes the initial specification (this repository). The specification is then ratified by the first nodes through sortition-selected assemblies.
- **Iterative bootstrap**: Start with a minimal rule set (just the axioms) and let the first nodes build up norms through the governance process.
- **Honest acknowledgment**: The bootstrap is inherently non-democratic. The founders' legitimacy comes not from selection but from the fact that the system they create will replace them.

### Related Axioms

Axiom 2 (Sortition Selection), Axiom 6 (Immutability of Axioms)

---

## 2. Coexistence with Existing States

### Description

PolOS nodes will exist within the jurisdiction of existing nation-states. These states will not recognize PolOS governance as legitimate. Members of PolOS nodes are simultaneously citizens of existing states, subject to their laws.

### Why It Matters

If PolOS norms conflict with state law, members face legal jeopardy. If PolOS defers to state law in all cases, it is merely a social club, not a governance system.

### Current Thinking

- **Parallel governance**: PolOS governs what existing states do not (community decisions, resource allocation within the community, internal dispute resolution).
- **Gradual expansion**: As PolOS proves effective, it takes on more governance functions — not by confrontation, but by demonstrating superior outcomes.
- **Legal wrappers**: PolOS nodes incorporate as cooperatives, associations, or other legal entities recognized by existing states, using PolOS governance internally.
- **Special economic zones**: Negotiate with existing states for zones of governance autonomy (precedent: charter cities, free zones).

### Related Axioms

Axiom 5 (Subsidiarity), Axiom 7 (Right of Exit)

---

## 3. Physical Anchoring

### Description

Governance ultimately requires physical infrastructure: buildings, roads, utilities, defense. PolOS is a digital governance system. How does it connect to the physical world?

### Why It Matters

A governance system that cannot provide physical security, infrastructure, or public goods is incomplete. Digital governance without physical anchoring is a game, not a government.

### Current Thinking

- **Start digital, grow physical**: Begin with purely digital governance (community decisions, resource allocation). Gradually acquire physical infrastructure as nodes grow.
- **Partnership model**: PolOS nodes partner with existing physical infrastructure providers (municipalities, cooperatives) while maintaining governance autonomy.
- **Territorial nodes**: Some nodes may be geographically defined (a neighborhood, a village), giving them natural physical anchoring.

### Related Axioms

Axiom 1 (Sovereign Identity — physical verification), Axiom 5 (Subsidiarity)

---

## 4. Intergenerational Governance

### Description

How does PolOS handle decisions that affect future generations? Current members can vote on norms that bind people who don't yet exist and cannot participate.

### Why It Matters

Climate policy, infrastructure investment, debt — many governance decisions have consequences that extend far beyond the current generation. Sortition selects from the current population, which may systematically underweight future interests.

### Current Thinking

- **Future generations ombudsperson**: A sortition-selected role specifically tasked with representing future interests.
- **Temporal impact assessment**: Norms with long-term consequences require a formal assessment of intergenerational impact.
- **Sunset clauses**: All norms with intergenerational impact automatically expire and must be re-ratified by future governance bodies.
- **Discount rate constraints**: The norm engine could enforce a maximum discount rate for future impacts.

### Related Axioms

Axiom 4 (Transparency — intergenerational audit trail), Axiom 5 (Subsidiarity — temporal subsidiarity?)

---

## 5. Physical Security Without Violence Monopoly

### Description

Existing states claim a monopoly on legitimate violence (Weber's definition). PolOS does not claim this monopoly. How does it handle physical security — crime, external threats, enforcement of norms?

### Why It Matters

Without enforcement capability, norms are suggestions. Without security, members are vulnerable. This is perhaps the hardest problem in the entire project.

### Current Thinking

- **Community-based security**: Sortition-selected security coordinators, community watch programs, restorative justice.
- **Outsourced enforcement**: For serious crimes, defer to existing state law enforcement (pragmatic compromise).
- **Norm compliance through exit**: The primary enforcement mechanism is social — if you violate norms, the community can exclude you (but cannot use violence). Right of exit (Axiom 7) works both ways.
- **Graduated sanctions**: Ostrom's design principles for commons governance include graduated sanctions — start with warnings, escalate to exclusion.

### Related Axioms

Axiom 7 (Right of Exit — the ultimate sanction is exclusion), Axiom 4 (Transparency — social accountability)

---

## 6. AI Norm Engine Alignment

### Description

Layer 5 (Continuous Audit) uses AI to analyze governance activity. Layer 3 (Norm Engine) may use AI to assist in translating natural language proposals into formal DSL. How do we ensure these AI systems are aligned with the axioms and the community's values?

### Why It Matters

AI systems can introduce bias, make errors, or be manipulated. If the AI norm interpreter systematically misinterprets proposals, it effectively controls governance. If the AI auditor has blind spots, violations go undetected.

### Current Thinking

- **AI as advisor, not decider**: AI systems recommend; humans (sortition-selected) decide.
- **Multiple independent AI systems**: No single AI model. Multiple models from different providers, with disagreements flagged for human review.
- **Formal verification of AI outputs**: AI-generated formal norms are verified against the axioms before deployment.
- **AI audit by sortition panels**: Randomly selected citizens review AI behavior and can override AI recommendations.
- **Open-source requirement**: All AI models used in PolOS must be open-source and auditable.

### Related Axioms

Axiom 8 (Norm Determinism — AI outputs must be deterministic for the same input), Axiom 4 (Transparency — AI reasoning must be auditable)

---

## 7. Economic Model

### Description

How does the system fund itself? Governance requires resources: infrastructure, compensation for sortition-selected officials, AI systems, cryptographic infrastructure.

### Why It Matters

Without a sustainable economic model, PolOS depends on donations or external funding — which creates dependency and potential influence.

### Current Thinking

- **Membership contributions**: Nodes collect contributions from members (amount set by the node's own governance).
- **Service fees**: Aggregations charge nodes for inter-node services.
- **Commons-based production**: Nodes can collectively produce goods and services, with revenue funding governance.
- **Grants and donations**: For the bootstrap phase, external funding may be necessary (with full transparency).
- **No taxation power**: PolOS cannot compel contributions. All funding is voluntary (consistent with Axiom 7 — right of exit).

### Related Axioms

Axiom 7 (Right of Exit — funding must be voluntary), Axiom 4 (Transparency — all finances public)

---

## 8. The Competence Problem

### Description

Sortition selects randomly, not by competence. A randomly selected council may include people with no relevant expertise. How does the system ensure competent governance?

### Why It Matters

This is the most common objection to sortition. If randomly selected citizens make worse decisions than elected experts, the system fails regardless of its democratic legitimacy.

### Current Thinking

- **Deliberation**: Research consistently shows that citizens, given time and information, make decisions comparable to or better than elected officials. The key is structured deliberation with expert input.
- **Expert advisors**: Sortition-selected bodies have access to expert advisors (who advise but do not decide).
- **Training period**: Selected officials receive training before assuming duties.
- **Collective intelligence**: Large, diverse groups consistently outperform small, homogeneous expert groups (Surowiecki's "Wisdom of Crowds" conditions: diversity, independence, decentralization, aggregation).
- **Empirical evidence**: Citizens' assemblies in Ireland, France, and British Columbia produced sophisticated policy recommendations — evidence that competence is not the bottleneck.
- **Counter-argument**: Elected officials are not selected for competence either. They are selected for electability — a different trait entirely.

### Related Axioms

Axiom 2 (Sortition Selection — the axiom this problem challenges directly)

---

## 9. The Scale Problem

### Description

Direct democracy (everyone votes on everything) doesn't scale. Sortition-based governance (randomly selected councils) scales better, but how far? Can PolOS govern a million people? A hundred million?

### Why It Matters

If PolOS only works for small communities, it's a commune, not a political system. The ambition is to provide governance at any scale.

### Current Thinking

- **Layered aggregation**: The L2 architecture allows recursive aggregation. A million people might be organized as 2,000 nodes of 500, aggregated into 40 districts of 50 nodes, aggregated into a single regional body.
- **Subsidiarity as scaling mechanism**: Most decisions are local (Axiom 5). Only genuinely inter-node issues escalate. This dramatically reduces the load on higher-level governance.
- **Statistical representation**: At large scale, sortition-selected bodies are statistically representative with high confidence (see sortition protocol §8.4).
- **Technology**: Digital infrastructure allows participation without physical presence, reducing the coordination cost of large-scale governance.
- **Open question**: At what scale does the system break down? Is there a maximum viable aggregation size? This requires empirical testing.

### Related Axioms

Axiom 2 (Sortition Selection), Axiom 5 (Subsidiarity)

---

## 10. Identity Without Centralization

### Description

Sovereign identity (L0) requires proving you are a unique human being. Current approaches (biometric scanning, government ID) require centralized infrastructure. How do you achieve Sybil resistance without centralization?

### Why It Matters

If identity requires a central authority, that authority becomes a single point of failure and control — violating the decentralization principle.

### Current Thinking

- **Web of trust**: Existing members vouch for new members. Sybil resistance comes from social verification.
- **Biometric-derived keys**: Use biometric data (iris, fingerprint) to derive a cryptographic key, then discard the biometric data. The key proves uniqueness without storing biometrics.
- **Proof of personhood protocols**: Projects like Worldcoin, Proof of Humanity, and BrightID are exploring this space. PolOS can learn from their successes and failures.
- **Hybrid approach**: Use multiple weak identity signals (social graph, biometric hash, behavioral patterns) that together provide strong Sybil resistance.
- **Open question**: Is truly decentralized Sybil-resistant identity possible? Or is some minimal centralization unavoidable?

### Related Axioms

Axiom 1 (Sovereign Identity)

---

## 11. Axiom Immutability vs. Adaptability

### Description

Axiom 6 states that the axioms cannot be modified by any process. But what if an axiom is discovered to be flawed, or if circumstances change so fundamentally that an axiom becomes harmful?

### Why It Matters

Immutability provides stability and prevents the system from being subverted from within. But it also prevents correction of errors. This is a genuine tension.

### Current Thinking

- **Axioms are structural, not policy**: The axioms constrain the structure of governance, not specific policies. Structural constraints change much less frequently than policies.
- **Fork as amendment**: If the axioms need to change, the system forks. A new version of PolOS is created with modified axioms. Members choose which version to follow. This is analogous to constitutional amendment, but without the risk of a majority abolishing minority protections.
- **Minimal axiom set**: The axioms are deliberately minimal. The less they constrain, the less likely they are to need modification.
- **Open question**: Is the fork mechanism sufficient? Or does it lead to fragmentation?

### Related Axioms

Axiom 6 (Immutability of Axioms — this problem is about Axiom 6 itself)

---

## How to Contribute to Open Problems

Each open problem is an invitation for research and deliberation. To contribute:

1. Open an issue referencing the specific problem number (e.g., "Open Problem #3: Physical Anchoring")
2. Provide evidence, analysis, or a concrete proposal
3. Reference relevant academic literature, historical precedents, or existing projects
4. Acknowledge tradeoffs — there are no perfect solutions to these problems
