# Day 24 — Circuit Breakers

**Module:** 2 — Reliability Engineering · **Week:** 1 · **Status:** ⬜ Not started

---

## 🎯 Learning Objectives

- Understand the three circuit breaker states: closed, open, half-open
- Implement a circuit breaker around the payment provider call
- Understand when circuit breakers help and when they don't
- Distinguish circuit breakers from retries and timeouts — they compose, not replace

## 💡 Why This Matters in Production

Timeouts bound a single call. Retries handle transient failures. Idempotency makes retries safe. But none of those patterns know when a dependency is *completely down* and retrying is just burning resources. Circuit breakers do — they stop hammering a dead dependency and let it recover.

"Fail fast" is a feature, not a bug.

## ⏱️ Session Breakdown

- **Theory (10 min):** Why retrying a dead dependency forever delays the failure and wastes resources
- **Practical (25 min):** Implement a circuit breaker for `payment-service`'s call to the external payment provider
- **Review (5 min):** Simulate provider outage; confirm the breaker opens and the system fails fast

## 📖 Key Concepts

```
CLOSED    → requests flow normally, failures counted
  ↓ (failure threshold exceeded, e.g. >50% errors in 10s window)
OPEN      → requests fail immediately, no call attempted
  ↓ (after cooldown period)
HALF-OPEN → a limited number of test requests allowed through
  ↓ success → CLOSED         ↓ failure → OPEN again

How the Week 1 patterns compose:
- Timeout (Day 21)      → bounds a single call's duration
- Retry + backoff (22)  → handles transient failures
- Idempotency (23)      → makes retries safe
- Circuit breaker (24)  → stops retrying a dependency that's clearly down
```

## 💻 Code Exercise

```javascript
// Redis-backed shared state — critical for horizontally scaled services.
// A per-process breaker doesn't protect the service; it just optimizes
// the one instance that's already seen the failures.
class CircuitBreaker {
  constructor(redis, { failureThreshold = 0.5, windowMs = 10_000, cooldownMs = 30_000 }) {
    // ...
  }

  async execute(fn) {
    const state = await this.getState();
    if (state === 'OPEN') throw new CircuitOpenError();
    try {
      const result = await fn();
      await this.recordSuccess();
      return result;
    } catch (err) {
      await this.recordFailure();
      throw err;
    }
  }
}
```

> [!NOTE]
> Shared Redis state is the non-obvious requirement here. A per-process breaker gives each instance its own independent failure counter — meaning the breaker won't open until every instance individually hits the threshold, by which point far more damage has been done.

## ✅ Success Criteria

- [ ] All three states implemented and observable (log or metric per transition)
- [ ] Shared state across instances — not per-process
- [ ] Can explain why timeout + retry + idempotency alone still isn't enough without a circuit breaker

---

## 📝 Review

_To be filled in after the exercise: what was learned, what was hard, self-score 1–5, and any item for spaced repetition._
