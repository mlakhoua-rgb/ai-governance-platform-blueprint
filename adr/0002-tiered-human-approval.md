# ADR 0002: Tiered Human Approval (L1–L5) Instead of Uniform Review

- **Status:** Accepted
- **Tier:** L4 (governance model)
- **Date:** 2026-01

## Context

A single review-and-approve step for all changes creates two opposite failure modes at once: it is **too heavy** for trivial changes (training people to rubber-stamp through volume) and **too light** for critical ones (a typo fixer and a risk-parameter change get the same scrutiny). As AI agents increase change volume by an order of magnitude, both failure modes amplify.

## Decision

Adopt a five-tier model where blast radius determines required authority:

- **L1** trivial/reversible → AI may merge after CI
- **L2** standard/reversible → peer review + CI gates
- **L3** significant → named senior approval + evidence pack + rollback plan
- **L4** critical/financial → two-person rule + decision record
- **L5** existential/irreversible → named executive + scheduled post-review

Enforcement is mechanical (branch protection, pipeline metadata, registry checks), classification is mandatory, ambiguous classification resolves *upward*, and authority maps to **named individuals** on a versioned roster.

## Consequences

- **Positive:** review effort concentrates where risk lives; accountability is attributable; the audit trail distinguishes decision classes automatically.
- **Negative:** classification overhead on every change; tier disputes; roster maintenance burden.
- **Mitigation:** intake tooling suggests tiers from change metadata; under-declaration is audited; roster changes are themselves L4 changes with review.

## Alternatives considered

- *Uniform four-eyes on everything* — rejected: doesn't scale with agent velocity; guarantees fatigue.
- *Risk-based sampling without tiers* — rejected: sampling is retrospective; L4/L5 decisions need prospective control.

---

*See [docs/ai-review-escalation-model.md](../docs/ai-review-escalation-model.md).*
