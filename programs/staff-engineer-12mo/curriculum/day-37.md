# Day 37 — Full Resilience Integration Challenge

**Module:** 2 — Reliability Engineering · **Week:** 4 · **Status:** ⬜ Not started

---

## 🎯 Objective

Verify all Module 2 patterns hold **simultaneously** under combined, realistic failure conditions.

## ⏱️ Session Breakdown

- **Setup (5 min):** Review the scenario; confirm all patterns are active
- **Challenge (30 min):** Run the integration scenario end-to-end
- **Review (5 min):** Document what held, what didn't, and why

## 🏗️ Integration Challenge

**Scenario:** Black-Friday-style load with multiple simultaneous failures.

Conditions to inject at the same time:
- Payment provider **degraded** (slow responses)
- One bulkhead-isolated dependency **down** (e.g. notification service)
- Burst traffic **exceeding rate limits**
- A batch of **duplicate purchase requests** from a retrying client

**Your task — verify all four guarantees hold:**

| Guarantee | Pattern | Expected behaviour |
|---|---|---|
| No duplicate charges | Idempotency keys | Same key → same result, no second charge |
| Circuit breaker recovers | Circuit breaker | Opens under load, recovers when provider returns |
| Excess load shed cleanly | Rate limiting | 429 + Retry-After, legitimate users unblocked |
| No duplicate emails, no lost jobs | DLQ + idempotent consumer | Jobs queue, drain, no duplicates |

## ✅ Success Criteria

- [ ] All four guarantees hold simultaneously under combined failure
- [ ] No duplicate purchases or charges
- [ ] No data loss — all valid purchases eventually processed
- [ ] Failures visible in Prometheus/Grafana/Jaeger without guessing

---

## 📝 Review

_To be filled in after the exercise: what was learned, what was hard, self-score 1–5, and any item for spaced repetition._
