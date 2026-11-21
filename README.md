# Praxis — a production case study

**A goal journal and accountability platform, self-hosted on two machines in a house in
Verona. Live with paying users. One engineer owns it end to end — database to deploy to
the 3 a.m. fix.**

This is the public architecture and operations story of [Praxis](https://praxisweb.xyz).
The application source is private (it is a commercial product); everything below is either
already public or published here with the operator's approval. Metrics are operator-reported,
not independently audited. No infrastructure secrets, credentials or customer data appear here.

*Snapshot: 2026-11-21.*

---

## 1. What it is, and who uses it

Praxis turns intentions into commitments someone else can see and confirm. Daily brief in
the morning, one-tap check-in, recap at night. Shared task delegation and peer verification
make a routine social: another person sees it and confirms it happened.

- **45 users, 3 on paid plans** (as reported on the public profile, 2026-11-21).
- A **clinical psychologist** runs it with a group of socially withdrawn young adults:
  daily self-care routines become shared, verified commitments.
- Installable PWA; **keeps working with no network**.
- Android through **3.7.0** in **7 languages**; SwiftUI iOS; Electron desktop; an **MCP
  server** that exposes the journal to agents.

The design thesis: *logged reality, not intention, is what revises the plan.*

## 2. Architecture (as shipped)

| Layer | Choice | Why |
|---|---|---|
| Web client | React 19, offline-first PWA | Client state is written **offline first** and reconciled deterministically: the same edit sequence converges to the same state on every device. No "lost update" roulette. |
| API | Express 5 (TypeScript) | One language across client and server; boring, debugable. |
| Database | Self-hosted Postgres (Supabase-compatible) | Own the data and the bill. Streaming replica on the second node. |
| Payments | Stripe | Live card payments, paid plans. |
| Mobile | Kotlin Android shell; SwiftUI iOS | The Android client now lives in the webapp's client directory — one client codebase, not per-platform silos (the standalone repo is archived with that note). |
| Desktop | Electron | Same web client, packaged. |
| Agents | MCP server | The journal is readable/actable by AI assistants without scraping. |

**The decision worth stealing:** offline-first is not a sync library bolted on later. The
write path is local-first by construction, and reconciliation is deterministic — convergence
is a property you can test, not a hope. The same discipline shows up in the operator's other
work (world models, verified replay): declare intent, measure reality, make the gap visible.

## 3. Operations

Two computers in a house in Verona:

| Host | Role | What it carries |
|---|---|---|
| **pansa** | Primary | Application, self-hosted Postgres, tunnel connector |
| **air** | Warm standby | Streaming database replica, second tunnel connector |

- **No cloud provider.** Public traffic arrives through tunnel connectors; the database
  replicates continuously between the two nodes.
- The person who fixes it at three in the morning is the person who built it.
- Extracted, reusable pieces are published: [tracker-libraries](https://github.com/ilPez00/tracker-libraries)
  (2,439 autocomplete records across 25 datasets, MIT, zero runtime dependencies) came out
  of Praxis and is released standalone.

## 4. Evidence you can check today

| Evidence | Where |
|---|---|
| Live product | [praxisweb.xyz](https://praxisweb.xyz) (HTTP 200 verified 2026-11-21) |
| Landing site source | [ilPez00/praxis-web](https://github.com/ilPez00/praxis-web) |
| Extracted data library | [ilPez00/tracker-libraries](https://github.com/ilPez00/tracker-libraries) |
| Android history | [ilPez00/praxis-android](https://github.com/ilPez00/praxis-android) (archived; superseded note in README) |
| Operator profile & metrics | [ilPez00](https://github.com/ilPez00/ilPez00) |

## 5. What is private, and why

The application source (webapp, backend, platform clients) is private because Praxis is a
commercial product with paying users. This case study exists so the engineering is
**independently assessable without the source**: architecture, operations model, design
decisions, and public artifacts you can run yourself. User counts and the clinical use case
are operator-reported and already public on the profile; they are not audited here.

## 6. What this demonstrates for a client

- One person can own a **real product end to end**: schema, API, offline sync, payments,
  mobile shells, deployment, replication, and the incident response.
- **Offline-first and deterministic** are buildable properties, not marketing words.
- **Self-hosting is operable**: two nodes, no cloud bill, continuous replication, and a
  repair story instead of a restart story.

## 7. Hire this engineer

Two fixed-scope offers (details on the [profile](https://github.com/ilPez00/ilPez00)):

1. **Systems audit & self-hosting migration** — one week: written map of what you run,
   failure modes, real cost, migration plan off platform bills. Report + runbook.
2. **Offline-first product build** — fixed milestones, spec to deploy: offline-capable
   PWA/Android shell with deterministic sync, on hardware you own.

[pezzingiovanniantonio@gmail.com](mailto:pezzingiovanniantonio@gmail.com) ·
[praxisweb.xyz/me](https://praxisweb.xyz/me) — answers within a day.