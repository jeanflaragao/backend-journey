# Day 28 — Rate Limiting Strategies

**Module:** 2 — Reliability Engineering · **Week:** 2 · **Status:** ⬜ Not started

---

## 🎯 Learning Objectives

- Compare token bucket vs. sliding window vs. fixed window
- Apply rate limiting per-user vs. per-IP vs. global
- Decide what to return to a rate-limited client (429 + `Retry-After`)
- Move rate limiting to the right enforcement point (`api-gateway`)

## 💡 Why This Matters in Production

Where you enforce limits matters as much as what the limits are. A per-service limiter means a client can hammer every service independently. A gateway-level limiter catches everything before it reaches any downstream service — one enforcement point, full coverage.

## ⏱️ Session Breakdown

- **Theory (10 min):** Enforcement point tradeoffs — API gateway vs. individual service
- **Practical (25 min):** Move rate limiting to `api-gateway`; add per-user limits for ticket purchases
- **Review (5 min):** Test with a script hammering the purchase endpoint

## 📖 Key Concepts

```
Response contract for rate-limited clients:
  HTTP 429 Too Many Requests
  Retry-After: <seconds>

Enforcement point tradeoffs:
  Per-IP at gateway        → cheap, coarse, defeated by NAT/proxies
  Per-user (authed) at GW  → better signal, requires auth early in pipeline
  Per-resource (per event) → business-specific, layered on top

Algorithm tradeoffs:
  Fixed window   → simple, cheap — burst at window boundary is the flaw
  Token bucket   → allows controlled bursts — good default for most APIs
  Sliding window → most accurate — highest Redis cost
```

## ✅ Success Criteria

- [ ] Rate limiting enforced at `api-gateway` — before reaching downstream services
- [ ] Per-user limit on ticket purchases implemented
- [ ] 429 response includes `Retry-After` header
- [ ] Can justify why gateway-level enforcement was chosen over per-service

---

## 📝 Review

_To be filled in after the exercise: what was learned, what was hard, self-score 1–5, and any item for spaced repetition._
