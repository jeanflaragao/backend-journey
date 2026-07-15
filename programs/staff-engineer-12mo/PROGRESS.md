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

**Day 21 — Timeouts and Retries** 🔄 *in progress*

- Opened with idempotency as a natural continuation from Module 1's final incident (DB pool exhaustion → Stripe timeout → client retry → duplicate charges)
- `HighPurchaseFailureRate` alert patched: failure-ratio expression, `rate(...) > 0.5` with sample-size guard, `for: 5m`
- Exercise assigned: extract p50, p99, and max observed latency for `payment-service` from Prometheus histograms + Jaeger traces, **before** setting any timeout (`timeout = p99 × 1.5` baseline)
- **Status:** awaiting latency numbers before configuring timeouts

**Days 22–25** ⬜ *not yet started*
Queue: retries with exponential backoff and jitter, idempotency keys, circuit breakers, Week 1 review

---

## 🔁 Spaced Repetition Tracker

| Item | Origin | Review Scheduled | Status |
|---|---|---|---|
| Tail-based sampling: OTel Collector is the decision point, not the API gateway | Module 1 | Day 40 | ⏳ Pending |

---

## 📚 Recommended Reading

Aligned to Module 2 — Reliability Engineering:

- *Designing Data-Intensive Applications* — Kleppmann, Ch. 8 & 9
- *Release It!* — Nygard, Ch. 4 & 5
- [Timeouts, retries, and backoff with jitter](https://aws.amazon.com/builders-library/timeouts-retries-and-backoff-with-jitter/) — Marc Brooker, AWS Builder's Library *(required before Day 22)*
