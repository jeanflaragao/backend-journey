# Day 40 — Module 2 Final Assessment

**Module:** 2 — Reliability Engineering · **Week:** 4 · **Status:** ⬜ Not started

---

## ⏱️ Session Breakdown

- **Theory quiz (10 min):** All Module 2 concepts + Module 1 spaced repetition check
- **Practical challenge (25 min):** Full reliability scenario
- **Reflection (5 min):** Module 2 graduation checklist

---

## 📝 Theory Assessment

Answer each **without notes**. Target: **8/10 to pass**.

| # | Question |
|:-:|---|
| 1 | How do you determine a timeout value — and why not just pick a round number? |
| 2 | Why is jitter required in retry backoff? |
| 3 | What problem do idempotency keys solve that retries alone don't? |
| 4 | Explain the three circuit breaker states and the purpose of each. |
| 5 | What's the difference between a bulkhead and a circuit breaker? |
| 6 | When would you choose graceful degradation over failing the request? |
| 7 | What's a dead-letter queue for? |
| 8 | Why must queue consumers be idempotent even with a reliable queue? |
| 9 | What's the purpose of chaos engineering, and what's the first step? |
| 10 | **Module 1 spaced repetition:** Where should the tail-based sampling decision be made in a distributed tracing setup, and why? |

**Score: __ / 10**

---

## 🏗️ Practical Challenge

Narrate or diagram the full TicketFlow purchase flow, hardened with every Module 2 pattern, explaining each pattern's role and what it **does not** solve on its own.

---

## 🔁 Module 1 Spaced Repetition — Tail-Based Sampling

> [!IMPORTANT]
> This was flagged as a recurring gap since Day 20. If you can't explain it clearly today, reschedule for Day 60.

**The answer:**
- Decision point is the **OTel Collector**, not the API gateway
- **Why:** the Collector sees the *full trace* (all spans from all services) before deciding whether to sample; the API gateway only sees the first span and cannot make an informed sampling decision

---

## 📊 Module 2 Graduation Checklist

```
Technical Skills (rate 1–10):
  [ ] Timeout / retry design    __ / 10
  [ ] Idempotency               __ / 10
  [ ] Circuit breakers          __ / 10
  [ ] Bulkheads & rate limiting __ / 10
  [ ] Queue-based resilience    __ / 10

  Average: __ / 10

Project Deliverables:
  [ ] Purchase flow hardened end-to-end (timeout, retry, idempotent, circuit breaker)
  [ ] Bulkhead isolation proven between at least 2 dependencies
  [ ] Rate limiting enforced at api-gateway
  [ ] Async email flow with DLQ and idempotent consumer
  [ ] 1 chaos experiment run and documented
  [ ] 1 reliability postmortem written

Interview Readiness:
  [ ] Can explain every pattern with a TicketFlow example
  [ ] Can explain what each pattern does NOT solve on its own
  [ ] Have a postmortem story ready for behavioral interviews

Career Progression:
  Before Module 2: 6/10
  After Module 2:  __ / 10
```

---

## 📝 Review

_To be filled in after the assessment: final score, what landed well, what to carry forward, and module 2 self-rating (1–10)._
