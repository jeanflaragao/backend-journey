# Module 2: Reliability Engineering
## Detailed 4-Week Lesson Plan

**Duration:** 4 weeks · Days 21–40
**Cadence:** 40 min/day · 5 days/week
**Total time:** ~13–14 hours
**Tech stack:** Node.js, TypeScript, Redis, Bull/BullMQ, PostgreSQL + Module 1 observability stack (pino, Prometheus, Grafana, OpenTelemetry, Jaeger)
**Project:** TicketFlow — `api-gateway` :3000 · `events-service` :3001 · `purchase-service` :3002 · `payment-service` :3003

---

## 🎯 Module Objectives

**Main goal:** Move from "it works on the happy path" to "it survives real production failure conditions" — timeouts, dependency outages, retry storms, duplicate requests, traffic spikes.

### By end of module you can:

1. Set timeouts from measured latency data, not guesses
2. Implement safe retries with exponential backoff and jitter
3. Guarantee idempotency on purchase and payment endpoints
4. Implement circuit breakers for external dependencies
5. Apply the bulkhead pattern to isolate failure domains
6. Implement rate limiting and graceful degradation
7. Build resilient async processing with dead-letter queues
8. Run a chaos experiment and write a reliability postmortem

### Skills progression

| Week | Focus |
|---|---|
| 1 | Timeouts, retries, idempotency, circuit breakers |
| 2 | Bulkheads, rate limiting, graceful degradation |
| 3 | Queue-based resilience (async retries, DLQs, idempotent consumers) |
| 4 | Chaos engineering, integration challenge, module assessment |

### Career impact

- **Interview:** can explain every major reliability pattern with a concrete TicketFlow example
- **Job level:** ready to own resilience for a production service, not just debug it after the fact
- **Differentiation:** most mid-level candidates can name these patterns; few can explain the tradeoffs and when *not* to use them

---

## 📅 Week 1: Timeouts, Retries & Idempotency

**Theme:** "Measure first. A timeout without data is a guess."

### Day 21 — Timeouts and Retries ✅

**Session breakdown:**
- Theory (10 min): why an unmeasured timeout is a production incident waiting to happen
- Practical (25 min): query Prometheus histogram for `payment-service`; cross-check Jaeger traces; compute p50/p99/max
- Review (5 min): confirm the proposed timeout value before writing any code

**Key concept:**
```
Cascade failure mechanics:
Missing timeout → connection held open → pool exhausted
  → new requests queue → clients retry → retry storm → total outage

Formula: timeout = p99 × 1.5
```

**Code exercise:**
```javascript
histogram_quantile(0.50, rate(payment_service_duration_seconds_bucket[5m]))
histogram_quantile(0.99, rate(payment_service_duration_seconds_bucket[5m]))
max(payment_service_duration_seconds_max)

const PAYMENT_TIMEOUT_MS = Math.round(p99 * 1.5); // derived, not guessed
```

**Success criteria:**
- [x] Query results shown before any code change
- [x] Timeout value = `p99 × 1.5`, not a round-number guess
- [x] No hardcoded timeout committed before the measurement step
- [x] Can explain the cascade-failure chain unprompted

---

### Day 22 — Retries with Exponential Backoff and Jitter

> [!TIP]
> Required reading before this session: [Timeouts, retries, and backoff with jitter](https://aws.amazon.com/builders-library/timeouts-retries-and-backoff-with-jitter/) — Marc Brooker, AWS Builder's Library.

**Session breakdown:**
- Theory (10 min): retry storms — why "just retry 3 times" amplifies an outage
- Practical (25 min): implement retry wrapper for `purchase-service → payment-service`
- Review (5 min): simulate 100 concurrent clients retrying with vs. without jitter

**Key concept:**
```
Naive retry:          all clients retry at once → thundering herd
Exponential backoff:  delay = base * 2^attempt  → spreads retries over time
Jitter:               randomize the delay        → prevents re-synchronizing

delay = min(cap, base * 2^attempt) * random(0.5, 1.5)
```

**Code exercise:**
```javascript
async function retryWithBackoff(fn, { maxAttempts = 4, baseMs = 200, capMs = 5000 }) {
  for (let attempt = 0; attempt < maxAttempts; attempt++) {
    try {
      return await fn();
    } catch (err) {
      if (attempt === maxAttempts - 1) throw err;
      const delay = Math.min(capMs, baseMs * 2 ** attempt);
      const jittered = delay * (0.5 + Math.random());
      await sleep(jittered);
    }
  }
}
```

**Success criteria:**
- [ ] Retries only on transient/retriable errors — not on 4xx business errors
- [ ] Jitter present, not just backoff
- [ ] Max attempts + max total wait explicitly bounded
- [ ] Can explain why retrying a non-idempotent operation without protection is dangerous

---

### Day 23 — Idempotency Keys & Safe Retries

**Session breakdown:**
- Theory (10 min): retrying a POST that already succeeded — but whose response was lost — causes duplicate side effects
- Practical (25 min): implement idempotency key middleware for `POST /api/tickets/purchase`
- Review (5 min): fire same request twice concurrently; confirm only one purchase created

**Key concept:**
```
Client sends: Idempotency-Key: <uuid>

Server:
  1. Key unseen → process, store (key → result) with TTL
  2. Key seen, done → return STORED result (no reprocess)
  3. Key seen, in-flight → 409 (race condition guard)
```

**Code exercise:**
```javascript
async function idempotencyMiddleware(req, res, next) {
  const key = req.headers['idempotency-key'];
  if (!key) return res.status(400).json({ error: 'Idempotency-Key required' });

  const existing = await redis.get(`idem:${key}`);
  if (existing === 'in-flight') return res.status(409).json({ error: 'Request already in progress' });
  if (existing) return res.status(200).json(JSON.parse(existing));

  await redis.set(`idem:${key}`, 'in-flight', 'EX', 86400, 'NX');
  res.locals.idempotencyKey = key;
  next();
}
```

**Success criteria:**
- [ ] Duplicate request with same key never creates a second purchase
- [ ] In-flight concurrent duplicate returns 409, not a second execution
- [ ] Redis TTL matches a documented, deliberate retry window
- [ ] Can explain in one sentence why Day 22's retry is dangerous without this

---

### Day 24 — Circuit Breakers

**Session breakdown:**
- Theory (10 min): "fail fast" as a feature — why retrying a dead dependency wastes resources
- Practical (25 min): implement circuit breaker for `payment-service` → payment provider call
- Review (5 min): simulate provider outage; confirm breaker opens and system fails fast

**Key concept:**
```
CLOSED → OPEN (on failure threshold) → HALF-OPEN (after cooldown) → CLOSED or OPEN

Composition:
  Timeout (21)      → bounds single call duration
  Retry (22)        → handles transient failures
  Idempotency (23)  → makes retries safe
  Circuit breaker   → stops retrying a dependency that's clearly down
```

> [!NOTE]
> The circuit breaker must use Redis-backed shared state. A per-process breaker doesn't protect a horizontally scaled service — each instance builds its own failure counter independently.

**Code exercise:**
```javascript
class CircuitBreaker {
  constructor(redis, { failureThreshold = 0.5, windowMs = 10_000, cooldownMs = 30_000 }) { /* ... */ }
  async execute(fn) {
    const state = await this.getState();
    if (state === 'OPEN') throw new CircuitOpenError();
    try {
      const result = await fn();
      await this.recordSuccess();
      return result;
    } catch (err) {
      await this.recordFailure();
      throw err;
    }
  }
}
```

**Success criteria:**
- [ ] All three states implemented and observable (log/metric per transition)
- [ ] Shared state across instances — not per-process
- [ ] Can explain why timeout + retry + idempotency alone isn't enough

---

### Day 25 — Week 1 Review & Assessment

**Comprehensive challenge (25 min):**
TicketFlow's payment provider is degrading intermittently. Harden the full purchase flow:
`idempotency key → timeout → retry with backoff/jitter → circuit breaker`

**Mock interview questions:**
1. How do you decide what timeout to set for a downstream call?
2. Why is jitter necessary in retry logic?
3. How do you make a payment endpoint safe to retry?
4. Walk me through the states of a circuit breaker and why each exists.
5. How do the four patterns compose, and what does each one NOT solve?

**Self-assessment:**
```
[ ] Can justify a timeout value with data           __ / 5
[ ] Can implement retry with backoff + jitter       __ / 5
[ ] Can implement idempotency keys correctly        __ / 5
[ ] Can implement and explain a circuit breaker     __ / 5
[ ] Can explain how all four patterns compose       __ / 5

Total: __ / 25   (target ≥ 20 to proceed)
```

---

## 📅 Week 2: Bulkheads & Rate Limiting

**Theme:** "Contain failure, don't let it spread."

### Day 26 — Bulkhead Pattern

**Key concept:**
```
Without bulkheads: shared pool → slow dependency A starves dependency B
With bulkheads:    per-dependency limits → A's slowness is contained to A
```

**Success criteria:**
- [ ] At least 2 dependencies isolated with separate limits
- [ ] Test proves isolation: inject latency in one, measure the other is unaffected
- [ ] Can explain bulkhead vs. circuit breaker (containment vs. fail-fast)

---

### Day 27 — Spaced Repetition: Module 1 Week 1 + Rate Limiting Intro

**Spaced repetition quiz (10 min, no notes):**
5-step debugging framework · structured logging vs. console.log · correlation IDs · log levels · what makes a log actionable

**Rate limiting intro:**
```
Token bucket:    allows controlled bursts — good default for most APIs
Sliding window:  most accurate — higher Redis cost
```

---

### Day 28 — Rate Limiting Strategies

**Key concept:**
```
Response: HTTP 429 + Retry-After: <seconds>

Enforcement point:
  Per-IP at gateway      → cheap, coarse, defeated by NAT
  Per-user at gateway    → better signal, requires auth early in pipeline
  Per-resource           → business-specific, layered on top
```

**Success criteria:**
- [ ] Rate limiting enforced at `api-gateway`
- [ ] Per-user limit on ticket purchases
- [ ] 429 + Retry-After contract correct
- [ ] Can justify enforcement point choice

---

### Day 29 — Graceful Degradation

**Key concept:**
```
Decision test: "If this call fails, can the user still accomplish their
primary goal?" Yes → candidate for graceful degradation.

Critical path (must fail loudly):   payment processing, inventory check
Non-critical (can degrade):         recommendations, trending events
```

> [!NOTE]
> Degradation must still be observable to *you* — log and metric every fallback, even if the user never sees it.

**Success criteria:**
- [ ] Fallback returns a sensible default, not an error
- [ ] Failure still logged/metriced
- [ ] Core purchase flow unaffected when degraded dependency is down

---

### Day 30 — Spaced Repetition: Module 1 Week 2 + Week 2 Assessment

**Spaced repetition quiz (10 min, no notes):**
4 golden signals · metric types · dashboard design · alert design principles · SLO/SLI/SLA

**Self-assessment:**
```
[ ] Bulkhead isolation          __ / 5
[ ] Rate limiting strategy      __ / 5
[ ] Graceful degradation        __ / 5
[ ] Module 1 Week 1–2 retained  __ / 5

Total: __ / 20   (target ≥ 16 to proceed)
```

---

## 📅 Week 3: Queue-Based Resilience

**Theme:** "Not everything needs to succeed synchronously."

### Day 31 — Introduction to Queue-Based Resilience

**Key concept:**
```
Rule: if the user doesn't need the result of this work to complete their
current action, it doesn't belong on the request path.

Purchase succeeds → email job enqueued → processed async with its own
retry policy, independent of the purchase request
```

---

### Day 32 — Dead Letter Queues & Poison Messages

**Key concept:**
```
Transient failure → retry helps      → exhaust retries → DLQ
Poison message   → retry won't help → DLQ after max attempts
```

> [!WARNING]
> A poison message without a DLQ sits at the head of the queue and blocks every healthy message behind it.

---

### Day 33 — Retry Policies for Async Jobs

**Key concept:**
```
Email confirmation:  5 attempts, backoff from 30s, cap 1h
  (user can wait; provider outages can last hours)

Inventory sync:      3 attempts, backoff from 5s, cap 60s
  (delay risks overselling)
```

---

### Day 34 — Idempotent Consumers

**Key concept:**
```
At-least-once delivery: job may be delivered and processed more than once.
The consumer — not the queue — is responsible for idempotency.
```

---

### Day 35 — Week 3 Review & Assessment

**Comprehensive challenge:**
Burst of duplicate job deliveries + temporarily broken email provider.
Verify: no duplicate emails · no lost jobs · DLQ alerts · auto-recovery.

**Self-assessment:**
```
[ ] Async processing decisions  __ / 5
[ ] DLQ handling                __ / 5
[ ] Per-job retry policies      __ / 5
[ ] Idempotent consumers        __ / 5

Total: __ / 20   (target ≥ 16 to proceed)
```

---

## 📅 Week 4: Chaos, Integration & Module Assessment

**Theme:** "Prove it survives failure — don't just assume it does."

### Day 36 — Chaos Engineering Basics

**Anatomy of an experiment:**
```
1. Define steady state   (what does "healthy" look like in metrics?)
2. State a hypothesis    (before injecting anything)
3. Inject the failure
4. Observe               (did actual match expected?)
5. Restore and document
```

---

### Day 37 — Full Resilience Integration Challenge

**Scenario:** Black-Friday-style load with simultaneous failures — degraded payment provider, isolated dependency down, burst exceeding rate limits, duplicate purchase requests from a retrying client.

All four guarantees must hold simultaneously:
- No duplicate charges
- Circuit breaker opens and recovers
- Rate limiting sheds load without blocking legitimate users
- Async jobs queue, drain, no duplicates

---

### Day 38 — Reliability Postmortem Practice

Write a full postmortem for a failure from Day 36 or 37. Root cause must name a specific missing Module 2 pattern. Action items must be specific, owned, and dated.

---

### Day 39 — Module 2 Comprehensive Review

All 9 patterns reviewed without notes, each with a TicketFlow example and a "what it does NOT solve" answer.

---

### Day 40 — Module 2 Final Assessment

**Theory quiz:** 10 questions · target **8/10** to pass (includes Module 1 tail-based sampling spaced repetition).

**Graduation checklist:**
```
Technical skills (rate 1–10):
  [ ] Timeout / retry design    __ / 10
  [ ] Idempotency               __ / 10
  [ ] Circuit breakers          __ / 10
  [ ] Bulkheads & rate limiting __ / 10
  [ ] Queue-based resilience    __ / 10
  Average: __ / 10

Project deliverables:
  [ ] Purchase flow hardened end-to-end
  [ ] Bulkhead isolation proven (≥ 2 dependencies)
  [ ] Rate limiting at api-gateway
  [ ] Async email with DLQ + idempotent consumer
  [ ] 1 chaos experiment documented
  [ ] 1 reliability postmortem written

Career progression:
  Before: 6/10 · After: __ / 10
```

---

## 🔄 Spaced Repetition Schedule

| Trigger | Content |
|---|---|
| Each session start (5 min) | Previous day's key concepts |
| Day 25 | Week 1 full review |
| Day 27 | Module 1 Week 1 recall |
| Day 30 | Module 1 Week 2 recall + Week 2 review |
| Day 35 | Week 3 full review |
| Day 40 | Full Module 1 review — tail-based sampling gap |
| Day 47 | Module 2 Week 1 review |
| Day 60 | Full Module 2 review |

---

## 🎯 Next: Module 3 — Database Performance (Days 41–60)

Query optimization, indexes, connection pooling, caching, migrations. The reliability patterns from this module (timeouts, circuit breakers, bulkheads) will directly protect the database work ahead.

---

*Last updated: 2026-07-16 · Current progress: Day 21 complete, Day 22 next*
