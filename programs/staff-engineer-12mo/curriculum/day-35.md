# Day 35 — Week 3 Review & Assessment

**Module:** 2 — Reliability Engineering · **Week:** 3 · **Status:** ⬜ Not started

---

## 🎯 Objectives

Verify all Week 3 patterns hold together under combined failure conditions.

## ⏱️ Session Breakdown

- **Review (5 min):** Quick recall of Days 31–34
- **Challenge (30 min):** Comprehensive Week 3 scenario
- **Self-assessment (5 min):** Score and flag gaps

## 🏗️ Comprehensive Challenge

**Scenario:** Simultaneous burst of duplicate job deliveries + temporarily broken email provider.

Verify:
1. No duplicate emails sent — **idempotent consumer** (Day 34)
2. No jobs lost — **DLQ** captures true failures (Day 32)
3. DLQ fires an alert when a job lands there
4. System recovers automatically and drains the queue once the provider is back

## 🎤 Mock Interview Questions

1. "When would you move something off the synchronous request path?"
2. "What's a dead-letter queue for, and what happens to jobs that land there?"
3. "Why must queue consumers be idempotent even if the queue promises reliable delivery?"

## 📊 Week 3 Self-Assessment

```
Self-rate each (1–5):

[ ] Can identify when to use async processing
[ ] Can implement DLQ handling correctly
[ ] Can design per-job retry policies with reasoning
[ ] Can implement idempotent consumers

Total: __ / 20

16–20  Excellent — ready for Week 4
12–15  Good — review weak areas
< 12   Revisit Week 3 before continuing
```

## ✅ Success Criteria

- [ ] All four verification points pass under the combined failure scenario
- [ ] Self-assessment completed and score recorded in `PROGRESS.md`
- [ ] Any gaps flagged for spaced repetition

---

## 📝 Review

_To be filled in after the exercise: what was learned, what was hard, self-score 1–5, and any item for spaced repetition._
