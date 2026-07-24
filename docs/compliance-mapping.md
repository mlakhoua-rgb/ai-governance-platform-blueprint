# Compliance Mapping

> How the engineering controls in this blueprint map to major frameworks. This is an engineering reference, **not** legal advice or a certification claim — see [DISCLAIMER.md](../DISCLAIMER.md).

## Why mapping matters

Regulators and auditors increasingly ask the same question in different vocabularies: *"Show me who decided, on what evidence, with what oversight, and how you can prove it."* If your controls live in architecture and CI — as this blueprint structures them — the answer is a query against your audit stream, not a slide deck.

## EU AI Act (readiness orientation)

For AI systems in scope of high-risk obligations, the Act expects risk management, data governance, technical documentation, logging, transparency, human oversight, accuracy/robustness. Mapping:

| EU AI Act theme | Blueprint control |
|---|---|
| Risk management system | Blast-radius tiering (L1–L5), invariant monitors, drift detection — continuous, not one-off |
| Data & data governance | Frozen validation datasets, leakage controls, input-staleness fail-closed behavior |
| Technical documentation | Registry evidence packs: spec, validation results, limitations, intended use per model/version |
| Record-keeping / logging | Append-only decision & inference log; audit-unavailability blocks decisions |
| Transparency to users | Decision packs: confidence, contributing factors, known limitations, counter-case |
| Human oversight | Tiered named authority; models advisory-only; override telemetry; kill switches |
| Accuracy, robustness, cybersecurity | Validation + shadow gates, calibration monitoring, SAST/secrets gates, least-privilege sandboxes |

## ISO/IEC 27001 (information security)

| Control area | Blueprint control |
|---|---|
| Access control | Named-authority roster (versioned), least-privilege agent sandboxes, two-person rule at L4 |
| Change management | Every change a proposal; CI gates + tiered approval; registry promotion with evidence |
| Operations security | Fail-closed config validation, invariant monitors, progressive deployment, drilled rollback |
| Logging & monitoring | Append-only tamper-evident audit stream; observability across all four planes |
| Supplier/dependency risk | Dependency & license scanning, allowlists, artifact provenance in registry |

## ISO 22301 (business continuity)

| Theme | Blueprint control |
|---|---|
| Continuity strategy | Tiered halts and safe states; degradation modes designed (read-only, advisory-degraded), not improvised |
| Exercising & testing | Chaos drills on kill switches, rollback rehearsals, incident simulations with the decision-support posture switch |
| Incident response | Incident command integration: pre-authorized emergency tiers, automatic timeline capture, 24h retrospective records |

## GDPR (where personal data appears)

| Principle | Blueprint control |
|---|---|
| Purpose limitation | Intended/forbidden-use documentation per model; schema validation rejects out-of-scope fields |
| Accountability | Decision records with named humans; reconstructable inference history |
| Automated decision safeguards | No fully automated consequential decisions — humans commit at the appropriate tier; override is first-class |

## NIST (AI RMF / CSF orientation)

| Function | Blueprint control |
|---|---|
| Govern | Authority roster, tier model, policy-as-code in CI |
| Map / Measure | Validation gates, calibration & drift metrics, override analytics |
| Manage | Gated promotion, auto-demotion on drift, fail-closed degradation |
| Identify / Protect / Detect / Respond / Recover | SAST & secrets gates, sandboxing, invariant detection, halt tiers, rehearsed recovery |

## The meta-point

None of these mappings required a separate "compliance project." They are emergent properties of the architecture: proposals-not-changes, tiered named authority, evidence-attached promotion, append-only logging, and fail-closed defaults. **Compliance designed into the system costs a fraction of compliance reconstructed afterward** — and survives contact with an auditor.

---

**Related:** [ai-review-escalation-model.md](ai-review-escalation-model.md) · [fail-closed-patterns.md](fail-closed-patterns.md) · [human-decision-support.md](human-decision-support.md)
