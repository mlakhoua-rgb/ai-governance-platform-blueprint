# ADR 0003: ML Models Are Advisory; Promotion Requires Shadow Evidence

- **Status:** Accepted
- **Tier:** L4 (ML governance posture)
- **Date:** 2026-02

## Context

Forecasting models create pressure from two directions: the business wants their outputs acted on automatically (latency, scale), and engineering wants frequent promotion of improved models (iteration speed). Both pressures, if ungoverned, produce the same outcome: unaccountable automated decisions with no reconstructable basis.

## Decision

1. **Models never act.** Outputs populate decision-support queues; execution paths are reachable only through deterministic constraint checks and human decision at the appropriate tier.
2. **Promotion is L4 by default** — two-person rule, recorded decision.
3. **Shadow mode is mandatory** before promotion: candidates run on live data with zero effect for a defined period; promotion evidence must include shadow results against production and realized outcomes.
4. **Retraining creates a new candidate**, re-entering the full lifecycle. No silent weight refreshes.
5. **Drift breaches auto-demote** to shadow or advisory-degraded mode, with a human review item on a clock.

## Consequences

- **Positive:** every production inference is reconstructable (version + inputs + output + human decision); model risk is a managed lifecycle, not a hope; override telemetry measures human–model calibration honestly.
- **Negative:** slower model iteration than pure-ML teams; shadow infrastructure cost; disciplined registry hygiene required.
- **Mitigation:** evidence packs are auto-assembled so governance doesn't strangle cycle time; promotion SLAs keep the queue moving.

## Alternatives considered

- *Autonomous execution within hard limits* — rejected: limits bound the loss but not the accountability gap; "the model did it within limits" is not an answer to a regulator.
- *A/B promotion without shadow* — rejected: A/B exposes real outcomes to an unproven model; shadow gathers equivalent evidence at zero effect.

---

*See [docs/ml-forecasting-governance.md](../docs/ml-forecasting-governance.md).*
