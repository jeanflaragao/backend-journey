# Day 21 — Timeouts and Retries

**Module:** 2 — Reliability Engineering · **Week:** 1 · **Status:** 🔄 In progress

---

## 🎯 Learning Objectives

- Understand why timeouts must come from measured latency, not intuition
- Extract p50, p99, and max latency for `payment-service` from Prometheus and Jaeger
- Apply the baseline formula `timeout = p99 × 1.5`
- Understand the cascade failure chain that missing timeouts cause

## 💡 Why This Matters in Production

A timeout set by intuition is a guess wearing a config value's clothes. Set it too low and you kill slow-but-successful requests. Set it too high and a single slow dependency holds connections open until the whole pool is exhausted — which is exactly the failure mode from the Module 1 final scenario: DB pool exhaustion cascading from a slow Stripe call → timeout → client retry → duplicate charges.

## ⏱️ Session Breakdown

- **Theory (10 min):** Why an unmeasured timeout is a production incident waiting to happen
- **Practical (25 min):** Query Prometheus histogram for `payment-service`; cross-check real Jaeger traces; compute p50/p99/max
- **Review (5 min):** Confirm the proposed timeout value before writing any code

## 📖 Key Concepts

```
Cascade failure mechanics:
Missing timeout → connection held open → pool exhausted
  → new requests queue → queued requests also slow
  → clients retry → retry storm → total outage

Formula (starting baseline): timeout = p99 × 1.5
- Too tight (p50): kills legitimate slow-but-fine requests
- Too loose (max): barely better than no timeout
- p99 × 1.5 gives headroom for the tail without waiting forever
```

## 💻 Code Exercise

```javascript
// Extract latency with PromQL — not by reading raw histograms manually
histogram_quantile(0.50, rate(payment_service_duration_seconds_bucket[5m]))
histogram_quantile(0.99, rate(payment_service_duration_seconds_bucket[5m]))
max(payment_service_duration_seconds_max)

// Then, and only then, configure:
const PAYMENT_TIMEOUT_MS = Math.round(p99 * 1.5); // derived, not guessed
```

> [!WARNING]
> Do not configure any timeout before you have the numbers. The measurement step is the exercise.

## ✅ Success Criteria

- [ ] p50, p99, and max latency extracted — PromQL used shown
- [ ] Values cross-checked against Jaeger traces (not Prometheus alone)
- [ ] Timeout value proposed = `p99 × 1.5`, with reasoning documented
- [ ] No hardcoded timeout committed before the measurement step
- [ ] Can explain the cascade-failure chain unprompted

---

## 📝 Review

_To be filled in after the exercise: what was learned, what was hard, self-score 1–5, and any item for spaced repetition._
