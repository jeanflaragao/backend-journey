# Staff Engineer 12-Month Program — Roadmap

240 study days · 40 min/day · 5 days/week · 48 weeks

> [!NOTE]
> This file is the **master checklist**. `PROGRESS.md` holds the narrative log. `CONTEXT.md` holds only the current pointer.

**Legend:** ✅ done · 🔄 in progress · ⬜ not started · 📝 lesson plan not yet written

---

### Overall Progress

```
█░░░░░░░░░░░░░░░░░░░  8.75%  (21 / 240 days)

Module 1  ████████████████████  20/20  ✅
Module 2  █░░░░░░░░░░░░░░░░░░░   1/20  🔄
Modules 3–12  ░░░░░░░░░░░░░░░  0/200  ⬜
```

---

## Module 1 — Production Debugging & Observability (Days 1–20) ✅

**Result:** 6/10 (target 5/10)

### Week 1 — Debugging Methodology & Structured Logging

- [x] Day 01 — The Debugging Mental Model
- [x] Day 02 — TicketFlow Foundation + Request Tracing
- [x] Day 03 — Log Levels & Context Enrichment
- [x] Day 04 — Production Log Analysis
- [x] Day 05 — Week 1 Review & Assessment

### Week 2 — Metrics, Monitoring & Alerting

- [x] Day 06 — Metrics Fundamentals
- [x] Day 07 — Building Dashboards
- [x] Day 08 — Alerting Strategy
- [x] Day 09 — CloudWatch & AWS Monitoring
- [x] Day 10 — Week 2 Review & Monitoring Challenge

### Week 3 — Distributed Tracing

- [x] Day 11 — Distributed Tracing Fundamentals
- [x] Day 12 — Multi-Service Tracing
- [x] Day 13 — Performance Analysis with Traces
- [x] Day 14 — Advanced Tracing Patterns
- [x] Day 15 — Week 3 Review & Tracing Challenge

### Week 4 — Incident Response & Postmortems

- [x] Day 16 — Incident Response Framework
- [x] Day 17 — Writing Postmortems
- [x] Day 18 — Incident Simulation
- [x] Day 19 — Observability Best Practices
- [x] Day 20 — Module 1 Final Assessment

> [!NOTE]
> Spaced repetition flagged: tail-based sampling decision point (OTel Collector, not the API gateway) → review scheduled **Day 40**.

---

## Module 2 — Reliability Engineering (Days 21–40) 🔄

**Lesson plan:** [`plans/module-02.md`](plans/module-02.md)

### Week 1 — Timeouts, Retries & Idempotency

- [x] Day 21 — Timeouts and Retries
- [ ] Day 22 — Retries with Exponential Backoff and Jitter
- [ ] Day 23 — Idempotency Keys & Safe Retries
- [ ] Day 24 — Circuit Breakers
- [ ] Day 25 — Week 1 Review & Assessment

### Week 2 — Bulkheads & Rate Limiting

- [ ] Day 26 — Bulkhead Pattern
- [ ] Day 27 — Spaced Repetition: Module 1 Week 1 + Rate Limiting Intro
- [ ] Day 28 — Rate Limiting Strategies
- [ ] Day 29 — Graceful Degradation
- [ ] Day 30 — Spaced Repetition: Module 1 Week 2 + Week 2 Assessment

### Week 3 — Queue-Based Resilience

- [ ] Day 31 — Introduction to Queue-Based Resilience
- [ ] Day 32 — Dead Letter Queues & Poison Messages
- [ ] Day 33 — Retry Policies for Async Jobs
- [ ] Day 34 — Idempotent Consumers
- [ ] Day 35 — Week 3 Review & Assessment

### Week 4 — Chaos, Integration & Module Assessment

- [ ] Day 36 — Chaos Engineering Basics
- [ ] Day 37 — Full Resilience Integration Challenge
- [ ] Day 38 — Reliability Postmortem Practice
- [ ] Day 39 — Module 2 Comprehensive Review
- [ ] Day 40 — Module 2 Final Assessment + Spaced Repetition: full Module 1 review

---

## Module 3 — Database Performance (Days 41–60) ⬜

Query optimization, indexes, connection pooling, caching, migrations.
Lesson plan to be written before Module 2 ends.

---

## Module 4 — AWS Essentials (Days 61–80) ⬜

S3, EC2, ECS, RDS, Lambda, SQS, CloudWatch, IAM, VPC.

---

## Module 5 — System Design Fundamentals (Days 81–100) ⬜

API design, caching strategies, queues, load balancing, webhooks.

---

## Module 6 — Payment Processing & Webhooks (Days 101–120) ⬜

Stripe integration, webhook processing, reconciliation, refunds.

---

## Module 7 — Race Conditions & Distributed Locks (Days 121–140) ⬜

Optimistic/pessimistic locking, Redis locks, transactions.

---

## Module 8 — Event-Driven Architecture (Days 141–160) ⬜

Event sourcing, CQRS, message queues, eventual consistency.

---

## Module 9 — System Design Interview Prep (Days 161–180) ⬜

8 system designs practiced, tradeoff analysis, mock interviews.

---

## Module 10 — Advanced Observability (Days 181–195) ⬜

Custom metrics, SLOs, alerting strategy, on-call, runbooks.

---

## Module 11 — Security & Performance (Days 196–210) ⬜

AuthN/AuthZ, rate limiting, injection prevention, load testing.

---

## Module 12 — Interview Intensive (Days 211–240) ⬜

Coding, system design, behavioral prep, portfolio + resume polish, applications sent.

---

## ✏️ How to Update This File

- Check `[x]` only after the day's **success criteria are verified against real code** — see `.ai/CLAUDE.md` § 3.
- When a module's lesson plan is written (`plans/module-XX.md`), replace the 📝 placeholder lines with real day titles.
- Keep the **Overall Progress** block in sync after each completed day.
