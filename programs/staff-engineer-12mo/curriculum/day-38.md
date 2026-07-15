# Day 38 — Reliability Postmortem Practice

**Module:** 2 — Reliability Engineering · **Week:** 4 · **Status:** ⬜ Not started

---

## 🎯 Learning Objectives

- Apply Module 1's postmortem format to a reliability-specific incident
- Write action items that name the specific pattern that was missing
- Practice blameless retrospective writing on a technical failure

## 💡 Why This Matters in Production

A postmortem that concludes "add more monitoring" is useless. One that traces the root cause to "no circuit breaker on the payment provider call, so the DB connection pool exhausted when the provider went down" gives you a specific, fixable action item — and a pattern to apply proactively everywhere else.

## ⏱️ Session Breakdown

- **Setup (5 min):** Pick one failure observed during Day 36 or Day 37
- **Writing (30 min):** Write a full postmortem using the template below
- **Self-check (5 min):** Review against the success criteria

## 📝 Postmortem Template

```markdown
# [Incident Title]

**Date:** YYYY-MM-DD | **Severity:** P0/P1/P2 | **Duration:** HH:MM → HH:MM

## Timeline
[Chronological events with timestamps]

## Root Cause
[One sentence]

### 5 Whys
1. Why did X happen?
2. Why did [answer to 1] happen?
[continue until systemic root cause]

## What Went Well
- [...]

## What Went Wrong
- [...]

## Action Items
| Action | Owner | Due |
|--------|-------|-----|
| [specific fix referencing the missing pattern] | | |
```

## ✅ Success Criteria

- [ ] Blameless tone maintained throughout
- [ ] Root cause traced to a **specific missing or misconfigured Module 2 pattern** (not "add monitoring")
- [ ] Action items are specific, actionable, owned, and dated

---

## 📝 Review

_To be filled in after the exercise: what was learned, what was hard, self-score 1–5, and any item for spaced repetition._
