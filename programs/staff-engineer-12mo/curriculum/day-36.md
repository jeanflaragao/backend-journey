# Day 36 — Chaos Engineering Basics

**Module:** 2 — Reliability Engineering · **Week:** 4 · **Status:** ⬜ Not started

---

## 🎯 Learning Objectives

- Understand the purpose of deliberately injecting failure
- Run a controlled experiment against TicketFlow
- Verify that the resilience patterns from Weeks 1–3 actually hold under real failure conditions

## 💡 Why This Matters in Production

You've implemented timeouts, retries, idempotency, circuit breakers, bulkheads, rate limiting, graceful degradation, DLQs, and idempotent consumers. But you haven't *proven* they work — you've only proven they compile. Chaos engineering is the discipline of breaking things on purpose, in a controlled way, to find out before production does.

## ⏱️ Session Breakdown

- **Theory (10 min):** Hypothesis-driven experiments, blast radius control, observing before assuming
- **Practical (25 min):** Run one controlled experiment (e.g. kill the payment provider mock mid-traffic, or inject 2s latency into one service)
- **Review (5 min):** Compare actual behavior against the hypothesis

## 📖 Key Concepts

```
Anatomy of a chaos experiment:
  1. Define steady state  (what does "healthy" look like in metrics?)
  2. State a hypothesis   (e.g. "circuit breaker opens within 10s; users
                           see a clean 503 instead of a hang")
  3. Inject the failure
  4. Observe              (did actual behavior match the hypothesis?)
  5. Restore and document gaps

Rule: start small — one dependency, one failure mode, controlled environment.
```

> [!IMPORTANT]
> State your hypothesis **before** running the experiment. If you observe first and hypothesize after, you're just calling a post-incident review a chaos experiment.

## ✅ Success Criteria

- [ ] Experiment has a clear hypothesis stated before running it
- [ ] Actual behavior documented and compared against the hypothesis
- [ ] Any gap between expected and actual resilience documented as an action item
- [ ] Can explain what "steady state" means and why you define it first

---

## 📝 Review

_To be filled in after the exercise: what was learned, what was hard, self-score 1–5, and any item for spaced repetition._
