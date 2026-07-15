# Day 26 — Bulkhead Pattern

**Module:** 2 — Reliability Engineering · **Week:** 2 · **Status:** ⬜ Not started

---

## 🎯 Learning Objectives

- Understand failure-domain isolation
- Prevent one slow dependency from exhausting shared resources needed by unrelated requests
- Implement per-dependency connection/concurrency limits in TicketFlow

## 💡 Why This Matters in Production

Even with circuit breakers and timeouts, a slow dependency can exhaust a shared thread pool or connection pool — starving completely unrelated, healthy request paths. The circuit breaker will eventually open, but by then the pool is gone and unrelated features have already failed. Bulkheads contain the blast radius before that happens.

## ⏱️ Session Breakdown

- **Theory (10 min):** Why a slow `notification-service` call shouldn't be able to starve `purchase-service` requests that never touch notifications
- **Practical (25 min):** Implement separate concurrency limits per downstream dependency in TicketFlow
- **Review (5 min):** Inject latency into one dependency; verify the others remain unaffected

## 📖 Key Concepts

```
Without bulkheads:
  Shared pool of 20 connections — all dependencies compete
  → slow dependency A holds 18/20 connections
  → dependency B (unrelated, healthy) can't get a connection either

With bulkheads:
  Dependency A: max 8 concurrent calls
  Dependency B: max 8 concurrent calls
  Dependency A going slow can hold at most its own 8 — B is unaffected
```

## 💻 Code Exercise

```javascript
// Simple per-dependency semaphore
const paymentLimiter = new ConcurrencyLimiter(8);
const notificationLimiter = new ConcurrencyLimiter(8);

async function callPaymentProvider(fn) {
  return paymentLimiter.run(fn);
}
```

> [!NOTE]
> Bulkhead vs. circuit breaker: these solve *different* problems and compose together. A bulkhead *contains* the blast radius of a slow dependency. A circuit breaker *stops calling* a dependency that's clearly down. You want both.

## ✅ Success Criteria

- [ ] At least 2 dependencies isolated with separate concurrency limits
- [ ] Test proves isolation: inject latency in one, measure the other is unaffected
- [ ] Can explain bulkhead vs. circuit breaker clearly

---

## 📝 Review

_To be filled in after the exercise: what was learned, what was hard, self-score 1–5, and any item for spaced repetition._
