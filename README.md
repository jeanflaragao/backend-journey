# Backend Journey

> 12-month structured path from mid-junior to senior/staff-level backend engineering.

> [!TIP]
> Starting a session? Read [`CONTEXT.md`](CONTEXT.md) first — it always has where things stand and what to do next.

---

## Why This Repo Exists

- **Single source of truth** for where the journey stands.
- **AI-agnostic:** plain Markdown, readable by Claude Code, ChatGPT, or any assistant — no lock-in.
- **Resumable by design:** read `CONTEXT.md` and any session (human or AI) knows exactly where to pick up.

---

## 🎓 Program: Staff Engineer (12-month)

**Goal:** Mid-Junior (3/10) → Senior-Ready (7-8/10) in 240 study days
**Cadence:** 40 min/day · 5 days/week · 48 weeks
**Project:** TicketFlow — [jeanflaragao/ticketflow](https://github.com/jeanflaragao/ticketflow)

| # | Days | Module | Lesson Plan | Status |
|:-:|---|---|---|---|
| 1 | 1–20 | Production Debugging & Observability | — | ✅ Complete — 6/10 |
| 2 | 21–40 | Reliability Engineering | [module-02.md](programs/staff-engineer-12mo/plans/module-02.md) | 🔄 Day 21 in progress |
| 3 | 41–60 | Database Performance | — | ⬜ Not started |
| 4 | 61–80 | AWS Essentials | — | ⬜ Not started |
| 5 | 81–100 | System Design Fundamentals | — | ⬜ Not started |
| 6 | 101–120 | Payment Processing & Webhooks | — | ⬜ Not started |
| 7 | 121–140 | Race Conditions & Distributed Locks | — | ⬜ Not started |
| 8 | 141–160 | Event-Driven Architecture | — | ⬜ Not started |
| 9 | 161–180 | System Design Interview Prep | — | ⬜ Not started |
| 10 | 181–195 | Advanced Observability | — | ⬜ Not started |
| 11 | 196–210 | Security & Performance | — | ⬜ Not started |
| 12 | 211–240 | Interview Intensive | — | ⬜ Not started |

---

## 🗂️ Structure

```
backend-journey/
├── CONTEXT.md                          ← start here every session
├── README.md                           ← you are here
│
├── .ai/
│   ├── CLAUDE.md                       ← instructions for Claude Code
│   └── AGENT_INSTRUCTIONS.md           ← instructions for any other AI
│
├── programs/
│   └── staff-engineer-12mo/
│       ├── ROADMAP.md                  ← 240-day master checklist
│       ├── PROGRESS.md                 ← narrative log, day by day
│       ├── plans/
│       │   └── module-02.md            ← detailed 4-week plan (one per module)
│       ├── curriculum/
│       │   └── day-XX.md               ← individual session files
│       └── decisions/
│           └── README.md               ← architecture decision records (ADRs)
│
└── projects/
    ├── README.md                       ← submodule instructions
    └── ticketflow/                     ← git submodule → jeanflaragao/ticketflow
```

---

## 📐 Conventions

- A day is only marked `[x]` in `ROADMAP.md` after its **success criteria are verified against real code** — not self-reported (see `.ai/CLAUDE.md` § 3).
- Module lesson plans (`plans/module-XX.md`) are written **before the module starts**, the same way `plans/module-02.md` was written before Module 2.
- Architecture decisions made in TicketFlow go in `decisions/` as ADRs.
- Lesson plans for modules 3–12 are generated closer to when those modules begin — see `ROADMAP.md` for the placeholder outline.
