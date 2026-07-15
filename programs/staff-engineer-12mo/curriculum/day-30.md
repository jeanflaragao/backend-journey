# Day 30 — Spaced Repetition: Module 1 Week 2 + Week 2 Assessment

**Module:** 2 — Reliability Engineering · **Week:** 2 · **Status:** ⬜ Not started

---

## ⏱️ Session Breakdown

- **Spaced repetition (10 min):** Module 1 Week 2 recall quiz — no notes
- **Assessment (25 min):** Comprehensive Week 2 challenge
- **Self-assessment (5 min):** Score and flag gaps

---

## 🔁 Spaced Repetition — Module 1, Week 2

Answer these **without notes**:

1. What are the 4 golden signals?
2. Name the main metric types (counter, gauge, histogram, summary) and give an example of each.
3. What makes a dashboard useful vs. a wall of charts?
4. What's the difference between SLO, SLI, and SLA?
5. What are the key properties of a good alert? *(name at least 3)*

_Score yourself after. Flag any gaps._

---

## 🏗️ Week 2 Comprehensive Challenge

**Scenario:** Simultaneous traffic spike + one dependency degrading.

Verify that:
1. **Bulkheads** contain the degrading dependency — other dependencies unaffected
2. **Rate limiting** sheds excess load at the gateway without blocking legitimate users
3. **Graceful degradation** handles the non-critical path cleanly — no 500 to the user

**Time:** 25 minutes

## 🎤 Mock Interview Questions

1. "How do bulkheads prevent cascading failure?"
2. "How would you rate-limit an API and what would you return to a client that hits the limit?"
3. "Give an example of graceful degradation you'd design for a real system."

## 📊 Week 2 Self-Assessment

```
Self-rate each (1–5):

[ ] Can implement bulkhead isolation
[ ] Can implement and justify a rate limiting strategy
[ ] Can identify and implement graceful degradation opportunities
[ ] Retained Module 1 Week 1–2 concepts (spaced repetition check)

Total: __ / 20

16–20  Excellent — ready for Week 3
12–15  Good — review weak areas
< 12   Revisit Week 2 before continuing
```

## ✅ Success Criteria

- [ ] Week 2 challenge passed with all three patterns active simultaneously
- [ ] Module 1 Week 2 recall: at least 4/5 without notes
- [ ] Self-assessment score recorded in `PROGRESS.md`

---

## 📝 Review

_To be filled in after the exercise: what was learned, what was hard, self-score 1–5, and any item for spaced repetition._
