# Day 27 — Spaced Repetition: Module 1 Week 1 + Rate Limiting Intro

**Module:** 2 — Reliability Engineering · **Week:** 2 · **Status:** ⬜ Not started

---

## ⏱️ Session Breakdown

- **Spaced repetition (10 min):** Module 1 Week 1 recall quiz — no notes
- **Theory (10 min):** Rate limiting as a reliability pattern, not just anti-abuse
- **Practical (15 min):** Implement a basic token bucket rate limiter on the purchase endpoint
- **Review (5 min):** Verify behavior under burst traffic

---

## 🔁 Spaced Repetition — Module 1, Week 1

Answer these **without notes**:

1. What are the 5 steps of the debugging mental model?
2. What's the difference between structured logging and `console.log`?
3. What is a correlation ID and why does every log line in a request need one?
4. Name the log levels in order and give a production example for each.
5. What makes a log "actionable" vs. just "informative"?

_Score yourself after. Flag any gaps._

---

## 📖 Rate Limiting — Key Concepts

Rate limiting protects *your own system's capacity*, not just prevents abuse. A legitimate burst from a large client can knock over a service just as effectively as an attack.

```
Token bucket:    tokens refill at a fixed rate; each request consumes one.
                 Requests rejected when the bucket is empty.
                 → Allows controlled bursts above the average rate.

Sliding window:  count requests in a rolling time window; reject over threshold.
                 More accurate than fixed windows — higher Redis cost.
                 → Better for strict per-second limits.
```

## 💻 Code Exercise

Implement a basic token bucket rate limiter on `POST /api/tickets/purchase`.

## ✅ Success Criteria

- [ ] Can answer at least 4/5 Module 1 Week 1 questions without notes
- [ ] Limiter correctly rejects requests once the bucket is empty
- [ ] Limiter correctly refills over time (requests succeed again after a pause)

---

## 📝 Review

_To be filled in after the exercise: what was learned, what was hard, self-score 1–5, and any item for spaced repetition._
