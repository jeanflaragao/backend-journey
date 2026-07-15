# Day 25 — Week 1 Review & Assessment

**Module:** 2 — Reliability Engineering · **Week:** 1 · **Status:** ⬜ Not started

---

## 🎯 Objectives

Consolidate the four Week 1 patterns, apply them together in a hardened purchase flow, and self-assess readiness before moving to Week 2.

## ⏱️ Session Breakdown

- **Review (10 min):** Quick recall of Days 21–24 — one key insight per day, no notes
- **Challenge (25 min):** Harden the full purchase flow end-to-end
- **Assessment (5 min):** Self-score against the checklist below

## 🏗️ Comprehensive Challenge

**Scenario:** TicketFlow's payment provider is degrading intermittently. Harden the full purchase flow using everything from this week:

```
Incoming purchase request
  → idempotency key checked          (Day 23)
  → call payment-service with
     timeout = p99 × 1.5             (Day 21)
  → retry with backoff + jitter
     on transient failures            (Day 22)
  → circuit breaker around
     payment provider call            (Day 24)
```

## 🎤 Mock Interview Questions

Practice these out loud — no notes:

1. "How do you decide what timeout to set for a downstream call?"
2. "Why is jitter necessary in retry logic?"
3. "How do you make a payment endpoint safe to retry?"
4. "Walk me through the states of a circuit breaker and why each exists."
5. "How do timeouts, retries, idempotency, and circuit breakers work together, and what does each one **not** solve on its own?"

## 📊 Week 1 Self-Assessment

```
Self-rate each (1–5):

[ ] Can justify a timeout value with measured data
[ ] Can implement retry with backoff + jitter
[ ] Can implement idempotency keys correctly
[ ] Can implement and explain a circuit breaker
[ ] Can explain how all four patterns compose

Total: __ / 25

20–25  Excellent — ready for Week 2
15–19  Good — review weak areas before moving on
< 15   Revisit Week 1 before continuing
```

## ✅ Success Criteria

- [ ] Full purchase flow hardened end-to-end with all four patterns active
- [ ] Self-assessment completed and score recorded in `PROGRESS.md`
- [ ] Any gaps flagged for spaced repetition

---

## 📝 Review

_To be filled in after the exercise: what was learned, what was hard, self-score 1–5, and any item for spaced repetition._
