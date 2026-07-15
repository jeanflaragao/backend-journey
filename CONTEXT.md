# Backend Journey — Context

> [!IMPORTANT]
> Read this file first, every session. Update it at the end — see `.ai/CLAUDE.md` § 5 for the exact steps.

---

## 📍 Snapshot

| Field | Value |
|---|---|
| **Program** | Staff Engineer (12-month) |
| **Module** | 2 — Reliability Engineering |
| **Week / Day** | Week 1 · Day 21 — Timeouts and Retries |
| **Project** | TicketFlow 🟢 Active |
| **Last updated** | 2026-07-15 |

---

## 📊 Where Things Stand

**Module 1** — Production Debugging & Observability ✅ complete — scored **6/10** (target 5/10)

**Module 2** — Reliability Engineering 🔄 in progress
- `HighPurchaseFailureRate` alert patched: failure-ratio expression, sample-size guard, `for: 5m`
- Day 21 exercise **in progress**: extract p50/p99/max latency for `payment-service` from Prometheus and Jaeger **before** configuring any timeout

→ Narrative log: [`PROGRESS.md`](programs/staff-engineer-12mo/PROGRESS.md)
→ Full checklist: [`ROADMAP.md`](programs/staff-engineer-12mo/ROADMAP.md)

---

## ⏭️ Immediate Next Step

Continue **Day 21**: run the PromQL queries against Prometheus to extract p50, p99, and max observed latency for `payment-service`. Cross-check 3–5 traces in Jaeger. The timeout value (`p99 × 1.5`) comes **after** the measurement — not before.

---

## 🔁 Open Items — Spaced Repetition

| Item | Status | Review |
|---|---|---|
| Tail-based sampling: decision point is the **OTel Collector**, not the API gateway | Recurring gap — flagged Module 1 | Day 40 |
| Idempotency | Being addressed — Day 23 | In progress |

---

## 🛠️ TicketFlow Stack

| | |
|---|---|
| **Stack** | Node.js · TypeScript · PostgreSQL · Redis · Docker |
| **Services** | `api-gateway` :3000 · `events-service` :3001 · `purchase-service` :3002 · `payment-service` :3003 |
| **Observability** | pino · Prometheus · Grafana · OpenTelemetry · Jaeger · OTel Collector (tail-based sampling) |
| **Repo** | [projects/ticketflow/](projects/ticketflow/) |

---

## 🔖 How to Resume Any Session

1. Read this file in full.
2. Read `programs/staff-engineer-12mo/PROGRESS.md` for the day-by-day narrative.
3. Read `programs/staff-engineer-12mo/curriculum/day-XX.md` for the current session.
4. If code review is needed, open `projects/ticketflow/` (git submodule).
5. Follow the teaching rules in `.ai/CLAUDE.md`.

---

## ✏️ How to Update This File

Update **only** these fields at the end of each session — do not rewrite history here:

- `Snapshot`: Module, Week/Day, Last updated
- `Where Things Stand`: brief current state
- `Immediate Next Step`
- `Open Items`: add / remove / reschedule rows
