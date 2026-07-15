# Day 33 — Retry Policies for Async Jobs

**Module:** 2 — Reliability Engineering · **Week:** 3 · **Status:** ⬜ Not started

---

## 🎯 Learning Objectives

- Apply Day 22's backoff/jitter principles to queue-level retries
- Configure per-job-type retry policies
- Understand how job-level retries differ from HTTP-level retries

## 💡 Why This Matters in Production

An async job retrying on failure has different constraints than an HTTP request retrying. The job's acceptable wait time is different (hours may be fine), the cost of a retry storm is lower (it's already async), and the right ceiling differs by how critical the job type is. One global policy doesn't fit all.

## ⏱️ Session Breakdown

- **Theory (10 min):** Why different job types need different retry policies — different SLAs, different failure costs
- **Practical (25 min):** Configure per-job-type retry policies (attempts, backoff, jitter) in Bull/BullMQ
- **Review (5 min):** Verify that email jobs and inventory-sync jobs have different, independently-reasoned policies

## 📖 Key Concepts

```
HTTP retry (Day 22): bounded by the client's timeout — must resolve in seconds
Job retry:           can spread over hours — total wait time can be much longer

Example policies:
  Email confirmation:  5 attempts, backoff from 30s, cap at 1h
    (user can wait; email provider outages can last hours)

  Inventory sync:      3 attempts, backoff from 5s, cap at 60s
    (must resolve fast — delay risks overselling)
```

## ✅ Success Criteria

- [ ] Retry policy (attempts, backoff, jitter) configured per job type
- [ ] At least 2 job types have different, documented policies with reasoning
- [ ] Can explain why a job's retry policy might differ from an HTTP call's retry policy

---

## 📝 Review

_To be filled in after the exercise: what was learned, what was hard, self-score 1–5, and any item for spaced repetition._
