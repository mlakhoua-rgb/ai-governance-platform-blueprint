# Human Critical Decision Support

> Designing the human side of the loop so that judgment is informed, recorded, and improvable.

## The problem with most "human-in-the-loop"

Typical implementations bolt an approve/reject button onto an AI pipeline and call it oversight. In practice this produces rubber-stamping (approval in seconds, at scale), alert fatigue, and — worst of all — decisions that cannot be reconstructed later because nobody recorded *why* the human agreed.

In a financially sensitive platform, the human decision is a **governed system component** with the same engineering rigor as any service: defined inputs, defined outputs, SLOs, and telemetry.

## Design: the decision pack

Every item reaching a human arrives as a structured **decision pack**:

```
┌─────────────────────────────────────────────────────┐
│ DECISION REQUIRED — Tier L4              SLA: 4h     │
│                                                     │
│ Recommendation   What the system proposes, plainly   │
│ Evidence         Metrics, forecast, contributing     │
│                  factors, historical analogues       │
│ Counter-case     Strongest argument AGAINST, plus    │
│                  what would make this wrong          │
│ Blast radius     What breaks if this is wrong;       │
│                  reversibility assessment            │
│ Alternatives     Other options considered, with      │
│                  why they rank lower                 │
│ Context          Related open items, recent          │
│                  incidents, drift flags              │
│                                                     │
│ [Approve] [Modify] [Reject] [Escalate]              │
│ Rationale required for Modify/Reject: ___________   │
└─────────────────────────────────────────────────────┘
```

Design choices that matter:

- **The counter-case is mandatory.** A decision pack that only argues *for* the recommendation trains humans to agree. Generating the strongest case against is an explicit system function, not a nice-to-have.
- **Modify is first-class.** Humans are not limited to binary approval; adjusting parameters is a normal, recorded outcome.
- **Rationale capture is lightweight but required** for non-approve outcomes — one sentence, structured. This is the single richest governance dataset the platform produces.
- **SLAs with escalation** — undecided items escalate up the authority roster, they never silently expire.

## Authority roster

Decision rights are assigned to **named individuals** per domain and tier, with named deputies and coverage calendars. The roster is versioned like code: changes to who may decide what are themselves L4 changes. See the [L1–L5 model](ai-review-escalation-model.md).

## Decision logging

Every decision — human, automated, or human-overriding-automation — is an append-only record containing:

- What was decided, by whom (name + tier), when
- The full decision pack as presented (the evidence *as seen*, not as later reconstructed)
- The rationale where provided
- Outcome linkage: subsequent events tied back to the decision

The log is **append-only and tamper-evident**; corrections are new records referencing old ones, never edits. This is what makes audit a query instead of an archaeology project.

## Closing the loop: decision quality review

Decisions are revisited on schedule and on trigger:

- **Scheduled sampling** — a percentage of L3–L5 decisions reviewed retrospectively each period
- **Outcome triggers** — any decision linked to an incident, loss event, or near-miss gets mandatory review
- **Calibration sessions** — periodic comparison of human overrides vs. model recommendations against realized outcomes, tuning both the models *and* the humans' trust in them

The goal is a system where both the AI and the humans get measurably better at deciding — and where "why did we do this?" always has an answer.

## Incident command integration

During incidents, the decision-support system switches posture: recommendation latency is prioritized over completeness, kill-switch and rollback actions are pre-authorized at defined tiers (see [fail-closed-patterns.md](fail-closed-patterns.md)), and every action feeds the incident timeline automatically. Post-incident, the timeline *is* the post-mortem skeleton — blameless by construction, because the system records what was known when, not who to blame.

---

**Related:** [ai-review-escalation-model.md](ai-review-escalation-model.md) · [ml-forecasting-governance.md](ml-forecasting-governance.md) · [compliance-mapping.md](compliance-mapping.md)
