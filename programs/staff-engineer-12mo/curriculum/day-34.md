# Day 34 — Idempotent Consumers

**Module:** 2 — Reliability Engineering · **Week:** 3 · **Status:** ⬜ Not started

---

## 🎯 Learning Objectives

- Understand at-least-once delivery and why consumers must be idempotent
- Apply Day 23's idempotency concept to queue consumers
- Handle duplicate job delivery safely

## 💡 Why This Matters in Production

Most queues guarantee at-least-once delivery, not exactly-once. A worker crash *after* processing a job but *before* acknowledging it causes the job to be redelivered. If the consumer isn't idempotent, the user gets two confirmation emails — or two charges. The queue won't save you; you must design for it.

## ⏱️ Session Breakdown

- **Theory (10 min):** At-least-once vs. exactly-once delivery — why exactly-once is harder than it sounds and why the consumer bears the responsibility
- **Practical (25 min):** Make the email confirmation consumer idempotent using a Redis deduplication key per job ID
- **Review (5 min):** Simulate duplicate job delivery; verify only one email action executes

## 📖 Key Concepts

```
At-least-once: job may be delivered and processed more than once
  (worker crash after processing but before acknowledging,
   or queue timeout + redelivery while processing is still running)

Exactly-once: theoretically possible, practically expensive and fragile

The consumer, not the queue, is responsible for idempotency.

Pattern: before processing, check if job ID has already been processed
  (Redis key with TTL). If yes → ack and return. If no → process and mark.
```

> [!NOTE]
> This is the same principle as Day 23 (idempotency keys on HTTP endpoints) applied one layer deeper. The pattern is identical; only the delivery mechanism is different.

## ✅ Success Criteria

- [ ] Duplicate job delivery proven safe via test — one action executed, not two
- [ ] Can explain at-least-once vs. exactly-once delivery
- [ ] Can explain why the consumer must handle duplication even if the queue promises reliable delivery

---

## 📝 Review

_To be filled in after the exercise: what was learned, what was hard, self-score 1–5, and any item for spaced repetition._
