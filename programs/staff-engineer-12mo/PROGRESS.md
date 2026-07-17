# Progress Log — Staff Engineer 12-Month Program

**Target:** Mid-Junior (3/10) → Senior-Ready (7-8/10) in 12 months
**Full checklist:** [`ROADMAP.md`](ROADMAP.md)

---

## Module 1 — Production Debugging & Observability ✅ Complete

**Result:** 6/10 (target 5/10)

| Skill | Score at end of module |
|---|---|
| Debugging | 6/10 |
| Observability | 6/10 |

**Deliverables shipped:**
- TicketFlow foundation with full observability stack: pino, Prometheus, Grafana, OpenTelemetry, Jaeger, OTel Collector (tail-based sampling)
- 2+ postmortems written

**Gap flagged for spaced repetition:** tail-based sampling decision point is the OTel Collector, not the API gateway → review scheduled Day 40

---

## Module 2 — Reliability Engineering 🔄 In Progress

### Week 1

**Day 21 — Timeouts and Retries** ✅ *complete* — self-score 3/5

- Opened with idempotency as a natural continuation from Module 1's final incident (DB pool exhaustion → Stripe timeout → client retry → duplicate charges)
- `HighPurchaseFailureRate` alert patched: failure-ratio expression, `rate(...) > 0.5` with sample-size guard, `for: 5m`
- `payment-service` had **no Prometheus instrumentation at all** going in — built a `payment_service_duration_seconds` histogram (labeled by `status`), wired timing into both the success and `502` exit paths, added `/metrics`, and added a scrape job for `:3003` (previously only the legacy monolith on `:3000` was scraped)
- First measurement: p50 = 2.33s, p99 = 4.92s — but cross-checking against Jaeger showed the **true max was only 3.03s**, meaning the Prometheus p99 exceeded the actual max observed value. Root cause: bucket boundaries (`3, 5`) had a 2s gap sitting right where the real "slow" cluster lived (~3.0-3.05s from `setTimeout` drift under load), so `histogram_quantile()`'s linear interpolation across that gap badly overestimated the tail
- Fixed by tightening buckets (`3, 3.1, 3.2, 3.5`) around the real cluster; re-measured p99 = 3.08s, consistent with Jaeger
- Proposed timeout: **4620ms** (`p99 × 1.5` using the corrected p99)
- Environment friction: `docker-compose` (hyphenated) not installed on this machine — had to use `docker compose` (v2 plugin) instead; repo also splits monitoring across two compose files (`docker-compose.yml` = Jaeger only, `docker-compose.monitoring.yml` = Prometheus/Grafana), which wasn't obvious and caused real delay
- **Spaced repetition flagged:** histogram bucket resolution directly determines quantile accuracy — always cross-check a fresh Prometheus quantile against raw trace/log data before trusting it, especially right after adding new instrumentation → review scheduled **Day 27**

**Days 22–25** ⬜ *not yet started*
Queue: retries with exponential backoff and jitter, idempotency keys, circuit breakers, Week 1 review

---

## 🔁 Spaced Repetition Tracker

| Item | Origin | Review Scheduled | Status |
|---|---|---|---|
| Tail-based sampling: OTel Collector is the decision point, not the API gateway | Module 1 | Day 40 | ⏳ Pending |
| Histogram bucket resolution determines quantile accuracy — cross-check fresh Prometheus quantiles against raw trace data | Day 21 | Day 27 | ⏳ Pending |

---

## 📚 Recommended Reading

Aligned to Module 2 — Reliability Engineering:

- *Designing Data-Intensive Applications* — Kleppmann, Ch. 8 & 9
- *Release It!* — Nygard, Ch. 4 & 5
- [Timeouts, retries, and backoff with jitter](https://aws.amazon.com/builders-library/timeouts-retries-and-backoff-with-jitter/) — Marc Brooker, AWS Builder's Library *(required before Day 22)*

---

## 📖 Independent / Complementary Reading

Books or content read on your own initiative — not part of any module's required list, tracked separately so it doesn't get lost in the day-by-day log.

| Title | Finished | Note |
|---|---|---|
| *The Pragmatic Programmer* — Hunt & Thomas | 2026-07-16 | Read alongside Module 2 |
