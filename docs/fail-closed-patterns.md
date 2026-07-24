# Fail-Closed Patterns

> The engineering embodiment of "when in doubt, stop safely." Config details withheld per [DISCLAIMER.md](../DISCLAIMER.md).

## Principle

A system that cannot verify it is operating safely must **stop in a safe state, alert, and wait for a human** — never improvise. In a financial context, "fail closed" means: no execution, no new exposure, no silent degradation. Features may become unavailable; safety properties may not.

## The patterns

### 1 · Configuration validation at boot — refuse to start broken
Every service validates its full configuration against a schema at startup: required keys present, values in range, referenced resources reachable, secrets resolvable. **Anything missing or invalid → the service does not start.** No defaults for safety-relevant parameters; a default risk limit is an oxymoron.

```pseudo
on_startup(config):
    result = schema_validate(config)
    if not result.ok:
        alert("SERVICE REFUSING START", result.errors)
        halt(exit_nonzero)          # orchestrator surfaces it; nothing half-runs
```

### 2 · Runtime invariant monitors — refuse to keep running broken
Invariants are checked continuously, not at deploy time: exposure within limits, state consistency (ledgers balance, queues drain, sequences unbroken), data freshness (inputs younger than their staleness bound). Breach → the affected capability transitions to a safe state automatically and a human item is created with a clock.

### 3 · Stale or invalid input → abstain, loudly
Forecasting and decision-support components treat stale data as a *stop* condition, not a "use it anyway" condition. The UI shows a degraded-evidence state; downstream automation that depends on the input halts. A system pretending to know is more dangerous than a system admitting it doesn't.

### 4 · Approval paths fail closed
If the approval service is down, the answer is **no** — not "allow and reconcile later." L3+ changes queued during an approval outage wait; they never auto-approve on timeout. Timeouts escalate to humans; they never resolve to yes.

### 5 · Circuit breakers and kill switches as tested services
- **Tiered halts**: freeze model promotion / halt execution / force read-only / full stop — each independently activatable
- **Dual trigger**: authorized humans at defined tiers, *and* the assurance plane automatically when defined invariants break
- **Drilled, not documented**: chaos exercises verify halts actually halt — including the ugly cases (mid-flight events, partially applied state). A kill switch that has never been pulled is a hypothesis
- **Restart is also gated**: resuming after a halt requires the same tier of approval as the halt required authority — no accidental auto-resume

### 6 · Deployment safety nets
Canary with SLO guard windows and automatic rollback; database migrations paired with rehearsed reverse migrations; feature flags defaulting to off for anything with financial effect. "Reversible" is demonstrated by drill, asserted never.

### 7 · Audit logging itself fails closed
If the append-only audit stream is unavailable, decision and execution paths **block** rather than proceed unlogged. An unaudited decision is treated as no decision. This is the pattern auditors ask about first, and the one most systems get wrong.

## The philosophy in one line

> Availability is negotiable. Integrity, auditability, and exposure control are not. When the system must choose, it chooses to stop — and to say so.

---

**Related:** [architecture-overview.md](architecture-overview.md) · [adr/0001](../adr/0001-fail-closed-configuration.md) · [human-decision-support.md](human-decision-support.md)
