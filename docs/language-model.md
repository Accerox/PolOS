# Language Architecture: The 4-Tier Model

## 1. Overview

In PolOS, **language is a display layer, not the canonical form**. Every norm, decision, and governance action exists first as a formal expression in the PolOS DSL (Domain-Specific Language). Human-readable text in any natural language is a **rendering** of that formal expression — like CSS rendering HTML, or a screen displaying binary data.

This design eliminates the ambiguity inherent in natural language governance. When two translations disagree, the formal DSL is authoritative. When a norm is "interpreted," the interpretation is a re-execution of the formal expression, not a human judgment call.

## 2. The 4 Tiers

```
┌─────────────────────────────────────────────────────────────┐
│  Tier 3: Cultural Adaptations                               │
│  Local idioms, examples, analogies, cultural context         │
│  Status: community-maintained, informational only            │
├─────────────────────────────────────────────────────────────┤
│  Tier 2: Community Translations                             │
│  Crowd-sourced translations in any language                  │
│  Status: flagged if outdated, not authoritative              │
├─────────────────────────────────────────────────────────────┤
│  Tier 1: Anchor Translations                                │
│  Authoritative human-readable translations                   │
│  Status: version-synced with Tier 0, reviewed by panels      │
├─────────────────────────────────────────────────────────────┤
│  Tier 0: Formal DSL                                         │
│  Canonical, machine-readable, deterministic                  │
│  Status: THE source of truth                                 │
└─────────────────────────────────────────────────────────────┘
```

### 2.1 Tier 0: Formal DSL (Canonical)

The formal DSL is the **only authoritative representation** of any norm or governance rule.

**Properties**:
- Machine-readable and machine-executable
- Deterministic: same input → same output, always
- Unambiguous: no room for interpretation
- Version-controlled: every change tracked with full history

**Example** (illustrative syntax — final DSL design is Phase 4):

```
norm meeting_recording {
    scope: node.meetings
    trigger: event(meeting.ended)
    condition: meeting.type in [council, assembly, committee]
    action: {
        require recording.published 
            within duration(hours: 24) 
            of meeting.ended
    }
    violation: {
        notify [auditor, facilitator]
        log violation(norm: this, meeting: trigger.meeting)
    }
    authority: assembly(supermajority: 2/3)
    effective: 2026-04-15
}
```

### 2.2 Tier 1: Anchor Translations (Authoritative)

Anchor translations are **official human-readable renderings** of Tier 0 norms in specific languages. They are:

- **Version-synced**: Every anchor translation is linked to a specific version of the Tier 0 norm. When the norm changes, the translation is flagged as outdated until updated.
- **Reviewed**: Anchor translations are reviewed by sortition-selected panels of native speakers.
- **Authoritative**: In case of ambiguity in a Tier 2 or Tier 3 rendering, the Tier 1 translation is the reference (but Tier 0 is the ultimate authority).

**Example** (English anchor translation of the above norm):

> **Norm: Meeting Recording Requirement**
>
> *Applies to*: All council meetings, full assembly meetings, and committee meetings.
>
> *Rule*: A recording of the meeting must be published within 24 hours of the meeting's end.
>
> *If violated*: The auditor and facilitator are notified. The violation is logged.
>
> *Approved by*: Full assembly, 2/3 supermajority.
>
> *Effective*: April 15, 2026.
>
> *Formal reference*: `norm.meeting_recording@v1.0`

**Initial anchor languages** (to be determined by community, but proposed):
- English
- Spanish
- Mandarin Chinese
- Arabic
- French
- Hindi

### 2.3 Tier 2: Community Translations (Crowd-Sourced)

Community translations extend coverage to any language. They are:

- **Crowd-sourced**: Any community member can contribute a translation.
- **Peer-reviewed**: Other speakers of the language review and approve.
- **Flagged if outdated**: When the Tier 0 norm changes, all Tier 2 translations are automatically flagged as potentially outdated.
- **Not authoritative**: Community translations are informational. In case of dispute, the Tier 1 anchor translation (or Tier 0 formal spec) governs.

**Quality indicators**:

```
TranslationStatus := {
    current: bool,           -- synced with latest Tier 0 version
    reviewed: bool,          -- peer-reviewed by ≥ 2 native speakers
    review_count: int,       -- number of reviews
    last_updated: Timestamp,
    tier0_version: Version,  -- which Tier 0 version this translates
    confidence: float        -- 0.0–1.0, based on reviews and freshness
}
```

### 2.4 Tier 3: Cultural Adaptations (Local Context)

Cultural adaptations go beyond translation to provide local context:

- **Idioms**: Explain concepts using local expressions and metaphors
- **Examples**: Provide examples relevant to the local community
- **Analogies**: Connect PolOS concepts to familiar local governance traditions
- **Context**: Explain how a norm relates to local customs, laws, or practices

**Example** (Spanish cultural adaptation of the meeting recording norm):

> **Norma: Grabación de Reuniones**
>
> Esta norma es similar al acta de reunión que se usa en las comunidades de vecinos en España, pero va más allá: requiere una grabación completa, no solo un resumen. Piensa en ello como el equivalente digital del acta notarial — un registro completo e inalterable de lo que se dijo y decidió.

Cultural adaptations are:
- **Informational only**: They do not modify the norm's meaning
- **Community-maintained**: Local communities create and maintain their own adaptations
- **Optional**: Not every norm needs a cultural adaptation in every locale

## 3. Version Synchronization

### 3.1 The Sync Problem

When a Tier 0 norm changes, all translations must be updated. But translations take time. During the update period, translations are out of sync with the canonical form.

### 3.2 Sync Protocol

```
When Tier 0 norm N is updated to version V:

1. All Tier 1 translations of N are flagged as OUTDATED
   - They remain visible but display a warning:
     "This translation is for version V-1. The norm has been updated."
   - The Tier 0 diff is displayed alongside the outdated translation

2. Tier 1 translation update is triggered:
   - AI generates a draft updated translation
   - Sortition-selected review panel reviews and approves
   - Once approved, the translation is marked CURRENT for version V

3. All Tier 2 translations of N are flagged as OUTDATED
   - Same warning as Tier 1
   - Community translators are notified

4. Tier 3 adaptations are flagged as REVIEW_NEEDED
   - Cultural adaptations may or may not need updating depending on 
     the nature of the change
```

### 3.3 CI-Enforced Sync

The version synchronization is enforced by continuous integration:

```
CI Pipeline:
1. On every Tier 0 change:
   - Flag all translations as outdated
   - Generate AI draft translations for Tier 1 languages
   - Open review tasks for sortition-selected panels
   - Track sync status in a public dashboard

2. Sync status dashboard:
   - For each norm: Tier 0 version, Tier 1 status per language, 
     Tier 2 status per language
   - Overall sync percentage
   - Average time to sync (per language)

3. Alerts:
   - If a Tier 1 translation is outdated for > 7 days: escalate
   - If a Tier 2 translation is outdated for > 30 days: flag for 
     community attention
```

## 4. Translation Review Process

### 4.1 Tier 1 Review

```
1. AI generates draft translation from Tier 0 change
2. Sortition-selected panel of 3 native speakers reviews:
   - Accuracy: Does the translation faithfully represent the Tier 0 norm?
   - Clarity: Is the translation clear and unambiguous?
   - Completeness: Are all elements of the norm translated?
3. Panel votes: unanimous approval required
4. If not unanimous: specific objections are addressed, re-reviewed
5. Approved translation is published with panel members' attestations
```

### 4.2 Tier 2 Review

```
1. Community member submits translation
2. At least 2 other native speakers review
3. Reviews are public (with reviewer identity protected by ZKP)
4. Translation is published with review count and confidence score
5. Ongoing: any community member can flag issues with a translation
```

## 5. Accessibility

### 5.1 Beyond Text

The language model is not limited to written text:

- **Audio rendering**: Norms can be rendered as audio (text-to-speech with human review)
- **Visual rendering**: Key concepts can be illustrated with diagrams, icons, and infographics
- **Simplified rendering**: A "plain language" version for each norm (lower reading level)
- **Sign language**: Video renderings in sign languages (community-contributed)

### 5.2 Literacy Considerations

PolOS must be accessible to people with varying literacy levels:

```
Rendering levels:
- Full: Complete norm text with all details
- Summary: Key points in simple language (reading level: grade 6)
- Visual: Infographic or diagram representation
- Audio: Spoken version with explanations
```

## 6. DSL Design Principles

The formal DSL (Tier 0) is designed with these principles:

1. **Minimal**: The smallest language that can express all needed governance rules
2. **Deterministic**: No ambiguity, no undefined behavior
3. **Composable**: Norms can reference and build on other norms
4. **Verifiable**: The DSL has formal semantics that allow machine verification
5. **Readable by experts**: While not natural language, the DSL should be readable by someone with basic programming literacy
6. **Translatable**: The DSL's structure should facilitate accurate translation to natural language

### 6.1 DSL Inspirations

| Language | What We Take From It |
|----------|---------------------|
| **Catala** | Legal domain modeling, scope-based rules |
| **Dhall** | Deterministic evaluation, no side effects |
| **Alloy** | Formal specification, constraint checking |
| **TLA+** | Temporal logic for governance processes |
| **SQL** | Declarative style for querying governance state |

### 6.2 DSL Non-Goals

The DSL is **not**:
- A general-purpose programming language
- Turing-complete (deliberately — to ensure termination and determinism)
- A replacement for natural language deliberation (deliberation happens in natural language; the result is formalized in the DSL)

## 7. Example: Full 4-Tier Rendering

### Tier 0 (Formal DSL)

```
norm council_term_limit {
    scope: node.governance.council
    invariant: forall member in council.members {
        consecutive_terms(member, council) <= 1
        AND cooldown_remaining(member, council) == 0 
            implies eligible(member, council.sortition)
    }
    on_violation: {
        reject selection(member, council)
        log axiom_violation(axiom: 3, context: this)
    }
}
```

### Tier 1 (English Anchor)

> **Norm: Council Term Limit**
>
> No council member may serve consecutive terms. After completing a term, a member must wait one full term (cooldown period) before becoming eligible for council selection again.
>
> If the sortition system selects someone who is in their cooldown period, the selection is rejected and an alternate is chosen. The incident is logged as an Axiom 3 violation.

### Tier 2 (Japanese Community Translation)

> **規範：評議会任期制限**
>
> 評議会メンバーは連続して任期を務めることはできません。任期を終えた後、再び評議会の選出対象となるには、1期分の冷却期間を待つ必要があります。
>
> 抽選システムが冷却期間中の人を選出した場合、その選出は却下され、補欠が選ばれます。この事象は公理3違反として記録されます。

### Tier 3 (Japanese Cultural Adaptation)

> この規範は、日本の地方自治体における首長の多選制限に似ていますが、より厳格です。PolOSでは、「多選」ではなく「連続」が制限されます。つまり、1期務めたら必ず1期休む必要があります。これは、特定の人物が権力を蓄積することを防ぐための構造的な保障です。

## 8. Open Questions

1. **DSL expressiveness vs. safety**: How expressive should the DSL be? More expressiveness means more governance scenarios can be formalized, but also more risk of unintended behavior.

2. **AI translation quality**: Can AI reliably translate formal DSL to natural language? What is the error rate? How do we catch errors?

3. **Translation latency**: How quickly can Tier 1 translations be updated after a Tier 0 change? Is 7 days acceptable?

4. **Minority languages**: How do we ensure coverage for languages with few speakers in the PolOS community?

5. **Legal standing**: If a PolOS norm is challenged in an existing legal system, which tier is presented? (Answer: Tier 0, with Tier 1 as explanatory material.)
