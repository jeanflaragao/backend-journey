# Day 21 — Timeouts and Retries

**Module:** 2 — Reliability Engineering · **Week:** 1 · **Status:** ✅ Complete

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

- [x] p50, p99, and max latency extracted — PromQL used shown
- [x] Values cross-checked against Jaeger traces (not Prometheus alone)
- [x] Timeout value proposed = `p99 × 1.5`, with reasoning documented
- [x] No hardcoded timeout committed before the measurement step
- [x] Can explain the cascade-failure chain unprompted

---

## 📝 Review

**What happened:** `payment-service` had zero Prometheus instrumentation going in — no histogram, no `/metrics` route, and `prometheus.yml` only scraped the legacy monolith on `:3000`. Built the histogram (`payment_service_duration_seconds`, labeled by `status`), wired `observe()` into both the success and `502` exit paths, added the `/metrics` route, and added a `payment-service` scrape job pointing at `:3003`.

First PromQL pass gave p50 = 2.33s, p99 = 4.92s. Cross-checking against Jaeger showed the **true max was only 3.03s** — meaning Prometheus's own p99 estimate was higher than the actual maximum observed value, a logical impossibility. Root cause: the histogram's original buckets (`...3, 5`) had a 2-second gap sitting exactly where the real "slow" cluster lived (~3.0–3.05s, from `setTimeout` drift under load), so `histogram_quantile()`'s linear interpolation across that gap produced a badly inflated estimate. Fixed by adding finer buckets (`3, 3.1, 3.2, 3.5`) around the real cluster and re-measuring — p99 came back at 3.08s, consistent with Jaeger.

**Final numbers:** p50 = 2.33s · p99 = 3.08s (post-fix) · max = 3.03s (Jaeger) → **timeout = 4620ms** (`p99 × 1.5`).

**What was hardest:** environment setup — `docker-compose` (hyphenated) wasn't installed, had to use `docker compose` (v2 plugin); and the repo has two separate compose files (`docker-compose.yml` = Jaeger only, `docker-compose.monitoring.yml` = Prometheus/Grafana), which caused real confusion about why "starting the stack" didn't bring Prometheus up.

**Self-score:** 3/5

**Spaced repetition candidate:** histogram bucket boundaries determine quantile accuracy — coarse buckets near where real data clusters cause `histogram_quantile()` to silently produce misleading tail estimates via linear interpolation; always cross-check a Prometheus-derived p99/p999 against raw trace data before trusting it, especially soon after adding new instrumentation with unverified bucket choices.
