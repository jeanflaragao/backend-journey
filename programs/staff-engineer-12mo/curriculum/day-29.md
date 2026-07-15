# Day 29 — Graceful Degradation

**Module:** 2 — Reliability Engineering · **Week:** 2 · **Status:** ⬜ Not started

---

## 🎯 Learning Objectives

- Design fallback behavior when a non-critical dependency fails
- Distinguish "must fail the request" from "can degrade gracefully"
- Implement a fallback for a non-critical TicketFlow feature

## 💡 Why This Matters in Production

Not every dependency failure should cascade into a user-facing error. If the recommendation service is down, the right response is to show the event page *without* recommendations — not a 500. Graceful degradation is about deciding in advance which failures are invisible to the user and what the fallback looks like.

## ⏱️ Session Breakdown

- **Theory (10 min):** Critical vs. non-critical paths — the decision test
- **Practical (25 min):** Implement graceful degradation for a non-critical TicketFlow call (e.g. "similar events" widget) with circuit breaker + fallback response
- **Review (5 min):** Simulate the dependency failing; confirm the core purchase flow is unaffected

## 📖 Key Concepts

```
Critical path (must fail loudly):
  Payment processing, inventory check
  → if this fails, the user cannot complete their goal

Non-critical path (can degrade silently):
  Recommendations, "trending events", non-essential enrichment
  → if this fails, the user can still complete their goal

Decision test: "If this call fails, can the user still accomplish their
primary goal?" Yes → candidate for graceful degradation.
```

> [!NOTE]
> Degradation should be *invisible to the user but visible to you*. Log and metric every fallback — silent degradation without observability is just a hidden failure.

## ✅ Success Criteria

- [ ] Fallback returns a sensible default, not an error, to the end user
- [ ] Failure is still logged and metriced — degradation is observable
- [ ] Core purchase flow is unaffected when the degraded dependency is down
- [ ] Can articulate the critical vs. non-critical distinction with a concrete TicketFlow example

---

## 📝 Review

_To be filled in after the exercise: what was learned, what was hard, self-score 1–5, and any item for spaced repetition._
