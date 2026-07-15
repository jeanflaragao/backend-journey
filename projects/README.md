# Projects

Real, standalone repos linked here as git submodules. Each one is
**portfolio-clean on its own** — no training material inside them. All training
context lives in this `backend-journey` repo instead.

---

## Active Projects

| Project | Repo | Stack | Role |
|---|---|---|---|
| TicketFlow | [jeanflaragao/ticketflow](https://github.com/jeanflaragao/ticketflow) | Node.js · TypeScript · PostgreSQL · Redis · OpenTelemetry · Docker | Main portfolio project for the Staff Engineer 12-month program. Evolves module by module. |

---

## Adding a New Project

```bash
git submodule add <repo-url> projects/<name>
```

Then:
1. Add a row to the table above.
2. Update the `Snapshot` table in `../CONTEXT.md`.
3. If it belongs to a program, link it from that program's `PROGRESS.md`.
