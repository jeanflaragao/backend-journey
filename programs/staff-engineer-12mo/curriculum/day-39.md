# Day 39 — Module 2 Comprehensive Review

**Module:** 2 — Reliability Engineering · **Week:** 4 · **Status:** ⬜ Not started

---

## 🎯 Objective

Consolidate all 9 Module 2 concepts before the final assessment. Identify any remaining weak areas.

## ⏱️ Session Breakdown

- **Review (35 min):** Work through all 9 topics below — explain each out loud or in writing, no notes
- **Flag (5 min):** Mark any gaps for extra review before Day 40

---

## 📖 Review Topics

For each topic: (1) what problem does it solve, (2) how did you implement it in TicketFlow, (3) what does it **not** solve on its own?

| # | Topic | Key question to answer without notes |
|:-:|---|---|
| 1 | **Timeout sizing** | Why `p99 × 1.5` and not a round number? |
| 2 | **Exponential backoff + jitter** | Why is jitter required, not optional? |
| 3 | **Idempotency keys (HTTP)** | What is the in-flight race condition and how do you handle it? |
| 4 | **Circuit breakers** | What triggers each state transition? Why shared Redis state? |
| 5 | **Bulkhead isolation** | How does it differ from a circuit breaker? |
| 6 | **Rate limiting** | Token bucket vs. sliding window. Where to enforce. What to return. |
| 7 | **Graceful degradation** | How do you decide what's critical vs. non-critical? |
| 8 | **Queue-based resilience** | DLQ, per-job retry policy, async vs. sync tradeoff |
| 9 | **Idempotent consumers** | At-least-once delivery — who's responsible for deduplication? |

## ✅ Success Criteria

- [ ] Can explain all 9 patterns without notes, each with a TicketFlow example
- [ ] Weak areas (if any) identified and flagged
- [ ] Confident going into Day 40

---

## 📝 Review

_To be filled in after the exercise: what was learned, what was hard, self-score 1–5, and any item for spaced repetition._
