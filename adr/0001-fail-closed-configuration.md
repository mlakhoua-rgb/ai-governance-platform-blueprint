# ADR 0001: Fail-Closed Configuration & Safety Defaults

- **Status:** Accepted
- **Tier:** L4 (architecture-wide safety posture)
- **Date:** 2026-01

## Context

Mission-critical financial software faces a recurring choice when configuration is missing, inputs are stale, or safety checks can't run: degrade gracefully (keep going with defaults/last-known state) or halt safely. Graceful degradation optimizes for availability. In a financial context, availability of an *unsafe* system is negative value — it converts uncertainty into unpriced risk.

## Decision

The platform fails closed:

1. **No defaults for safety-relevant configuration.** Services validate full config at boot and refuse to start on any violation.
2. **Stale/invalid inputs cause loud abstention**, never silent continuation with degraded data.
3. **Approval paths fail closed** — approval-service outage means "no", never "proceed and reconcile".
4. **Audit-log unavailability blocks decision and execution paths.** Unlogged decisions are treated as no decisions.
5. **Halts require the same tier of authority to resume as to trigger** — no auto-resume.

## Consequences

- **Positive:** unsafe states have zero dwell time; auditors can verify the posture from the audit stream; incident reviews start from complete records.
- **Negative:** more service refusals and blocked queues during infrastructure instability; requires investment in dependency redundancy (approval service, audit stream) so fail-closed doesn't become always-closed.
- **Mitigation:** the approval and audit services carry the highest SLOs in the platform; their unavailability is itself a top-tier alert.

## Alternatives considered

- *Graceful degradation with defaults* — rejected: defaults for safety parameters encode invisible policy decisions made by whoever wrote the constant.
- *Fail-open with after-the-fact reconciliation* — rejected: unreconstructable decisions are unacceptable in a regulated financial context.

---

*See [docs/fail-closed-patterns.md](../docs/fail-closed-patterns.md).*
