# Day 31 — Introduction to Queue-Based Resilience

**Module:** 2 — Reliability Engineering · **Week:** 3 · **Status:** ⬜ Not started

---

## 🎯 Learning Objectives

- Understand when to move work off the synchronous request path
- Set up Bull/BullMQ backed by Redis
- Move ticket confirmation email sending to an async queue

## 💡 Why This Matters in Production

If email sending is inline with the purchase request, a slow or down email provider makes every purchase slow or fail — even though the ticket was purchased successfully. The two concerns don't belong on the same request path. Queues decouple them.

## ⏱️ Session Breakdown

- **Theory (10 min):** What belongs on the request path and what doesn't — the rule of thumb
- **Practical (25 min):** Configure Bull/BullMQ with Redis; move ticket confirmation email to an async job
- **Review (5 min):** Confirm purchase succeeds even with the email worker stopped

## 📖 Key Concepts

```
Synchronous risk:
  email sending inline → slow email provider makes every purchase slow
  or fail, even though the purchase itself succeeded

Queue-based:
  purchase succeeds → email job enqueued → processed asynchronously
  with its own retry policy, independent of the purchase request

Rule of thumb: if the user doesn't need the result of this work to
complete their current action, it doesn't belong on the request path.
```

## ✅ Success Criteria

- [ ] Purchase endpoint no longer blocks on email sending
- [ ] Job visible in queue and processed by a worker
- [ ] Purchase still succeeds even when the email worker is stopped
- [ ] Can name at least 2 other TicketFlow operations that could move off the request path

---

## 📝 Review

_To be filled in after the exercise: what was learned, what was hard, self-score 1–5, and any item for spaced repetition._
