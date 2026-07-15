# Day 32 — Dead Letter Queues & Poison Messages

**Module:** 2 — Reliability Engineering · **Week:** 3 · **Status:** ⬜ Not started

---

## 🎯 Learning Objectives

- Understand what happens when a job fails permanently
- Implement a dead-letter queue (DLQ) for jobs that exhaust retries
- Prevent a single malformed ("poison") message from blocking the queue

## 💡 Why This Matters in Production

Without a DLQ, a job that will never succeed either gets retried forever (wastes resources, can block healthy jobs) or is silently dropped (data loss). Neither is acceptable. The DLQ is the "never lose a message, never block the queue" answer.

## ⏱️ Session Breakdown

- **Theory (10 min):** Transient failure vs. poison message — why the response to each is different
- **Practical (25 min):** Configure a DLQ for the email job queue; verify a malformed job doesn't block healthy ones
- **Review (5 min):** Confirm a log/alert fires when a job lands in the DLQ

## 📖 Key Concepts

```
Job fails → retried per policy → still fails after max attempts
  → moved to DLQ (not dropped, not retried forever)

DLQ purpose: nothing is lost, something alerts, manual or automated
  reprocessing is possible later.

Transient failure: retry helps      (e.g. email provider temporarily down)
Poison message:    retry won't help (e.g. malformed payload, consumer bug)
  → DLQ it after max attempts; don't let it clog the main queue
```

> [!WARNING]
> A poison message without a DLQ will sit at the head of a queue and block every healthy message behind it. One bad job can take down an entire async pipeline.

## ✅ Success Criteria

- [ ] Job exhausting retries lands in DLQ — not silently dropped
- [ ] A deliberately malformed job doesn't block other jobs from processing
- [ ] A log or alert fires when a job lands in the DLQ
- [ ] Can explain the difference between a transient failure and a poison message

---

## 📝 Review

_To be filled in after the exercise: what was learned, what was hard, self-score 1–5, and any item for spaced repetition._
