# Day 22 — Retries with Exponential Backoff and Jitter

**Module:** 2 — Reliability Engineering · **Week:** 1 · **Status:** ⬜ Not started

---

## 🎯 Learning Objectives

- Understand why naive immediate retries make outages worse
- Implement exponential backoff on the `purchase-service → payment-service` call
- Understand why jitter is required, not optional
- Set a sane retry ceiling (max attempts + max total wait)

## 💡 Why This Matters in Production

Retrying immediately and repeatedly is the textbook way to turn a partial outage into a total one. When all clients hit a failing service at the same moment, retry at the same moment, and do it again in lockstep, the recovering service never gets breathing room. Backoff buys it time; jitter prevents clients from re-synchronizing after the backoff.

> [!TIP]
> Required reading before this session: [Timeouts, retries, and backoff with jitter](https://aws.amazon.com/builders-library/timeouts-retries-and-backoff-with-jitter/) — Marc Brooker, AWS Builder's Library.

## ⏱️ Session Breakdown

- **Theory (10 min):** Retry storms — why "just retry 3 times" without backoff amplifies an outage
- **Practical (25 min):** Implement retry wrapper for the `purchase-service → payment-service` call
- **Review (5 min):** Simulate 100 concurrent clients retrying; observe the difference with/without jitter

## 📖 Key Concepts

```
Naive retry (no backoff): all clients retry at once → thundering herd
Exponential backoff: delay = base * 2^attempt → spreads retries over time
Jitter: randomize the delay → prevents clients from re-synchronizing

delay = min(cap, base * 2^attempt) * random(0.5, 1.5)   // full jitter variant
```

## 💻 Code Exercise

```javascript
async function retryWithBackoff(fn, { maxAttempts = 4, baseMs = 200, capMs = 5000 }) {
  for (let attempt = 0; attempt < maxAttempts; attempt++) {
    try {
      return await fn();
    } catch (err) {
      if (attempt === maxAttempts - 1) throw err;
      const delay = Math.min(capMs, baseMs * 2 ** attempt);
      const jittered = delay * (0.5 + Math.random());
      await sleep(jittered);
    }
  }
}
```

## ✅ Success Criteria

- [ ] Retries only on transient/retriable errors — not on 4xx business errors
- [ ] Jitter present, not just backoff
- [ ] Max attempts + max total wait explicitly bounded
- [ ] Can explain why retrying a non-idempotent operation without protection is dangerous *(bridge to Day 23)*

---

## 📝 Review

_To be filled in after the exercise: what was learned, what was hard, self-score 1–5, and any item for spaced repetition._
