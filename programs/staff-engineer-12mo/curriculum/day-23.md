# Day 23 — Idempotency Keys & Safe Retries

**Module:** 2 — Reliability Engineering · **Week:** 1 · **Status:** ⬜ Not started

---

## 🎯 Learning Objectives

- Understand why retries are unsafe without idempotency
- Implement idempotency keys on the purchase endpoint
- Handle the "in-flight duplicate request" race condition
- Connect this directly to the double-charge scenario from Module 1

## 💡 Why This Matters in Production

Day 22's retry logic is dangerous without this. If a POST to create a purchase succeeds on the server but the response is lost in transit, the client retries and creates a second purchase — a duplicate charge, a duplicate ticket. This is the exact failure from the Module 1 final incident. Idempotency keys are what make retries safe.

## ⏱️ Session Breakdown

- **Theory (10 min):** Retrying a POST that already succeeded — but whose response was lost — causes duplicate side effects unless the operation is idempotent
- **Practical (25 min):** Implement idempotency key middleware for `POST /api/tickets/purchase`
- **Review (5 min):** Fire the same request twice concurrently; confirm only one purchase is created

## 📖 Key Concepts

```
Client sends: Idempotency-Key: <uuid> header

Server behavior:
1. Key not seen before  → process request, store (key → result) with TTL
2. Key seen, completed  → return the STORED result, don't reprocess
3. Key seen, in-flight  → reject with 409 (race condition guard)

Storage: Redis with TTL matching the client's realistic retry window (e.g. 24h).
Key: idempotency key + endpoint + request hash (catches key reuse with different
payload — that's a client bug, not a retry).
```

## 💻 Code Exercise

```javascript
async function idempotencyMiddleware(req, res, next) {
  const key = req.headers['idempotency-key'];
  if (!key) return res.status(400).json({ error: 'Idempotency-Key required' });

  const existing = await redis.get(`idem:${key}`);
  if (existing === 'in-flight') {
    return res.status(409).json({ error: 'Request already in progress' });
  }
  if (existing) {
    return res.status(200).json(JSON.parse(existing));
  }

  await redis.set(`idem:${key}`, 'in-flight', 'EX', 86400, 'NX');
  res.locals.idempotencyKey = key;
  next();
}

// After the handler completes successfully:
await redis.set(`idem:${key}`, JSON.stringify(result), 'EX', 86400);
```

> [!WARNING]
> The in-flight guard (case 3) is not optional. Without it, two simultaneous requests with the same key can both pass the "key not seen" check and both execute.

## ✅ Success Criteria

- [ ] Duplicate request with the same key never creates a second purchase
- [ ] In-flight concurrent duplicate returns 409, not a second execution
- [ ] Redis TTL matches a documented, deliberate retry window
- [ ] Can explain in one sentence why Day 22's retry logic is dangerous without this

---

## 📝 Review

_To be filled in after the exercise: what was learned, what was hard, self-score 1–5, and any item for spaced repetition._
