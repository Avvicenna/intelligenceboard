# CONTEXT.md — intelligenceBoard
<!-- Static. Update only when stack or architecture changes. -->
<!-- Last updated: 2026-04-15 -->

## Project Identity

| Field | Value |
|-------|-------|
| Name | IntelligenceBoard |
| Package | `@claudesy/intelligenceboard` |
| Version | 0.1.0 |
| Type | Clinical Artificial Intelligence dashboard — standalone Next.js app |
| Author | Dr. Ferdi Iskandar (Claudesy) |
| Repo | `github.com/Claudesy/intelligenceBoard` (private) |
| License | MIT |
| Node | ≥20.9.0 |
| Package manager | pnpm ≥9 |

## Purpose

Indonesia-calibrated clinical intelligence platform for primary healthcare
facilities (Puskesmas, FKTP, PONED). Unifies EMR automation, CDSS, telemedicine,
voice Artificial Intelligence, and real-time operations into one decision surface
aligned with Kementerian Kesehatan reporting standards.

## Core Stack

| Layer | Technology | Version |
|-------|-----------|---------|
| Framework | Next.js (App Router) | 16.2.3 |
| UI | React | 19.2.3 |
| Styling | Tailwind CSS v4 | ^4.2.1 |
| ORM | Prisma | ^6.16.1 |
| Database | Neon PostgreSQL (serverless) | — |
| Real-time | Socket.IO | ^4.8.3 |
| Telemedicine | LiveKit | server ^2.15.0 / client ^2.17.2 |
| AI model | Google Gemini (`@google/generative-ai`) | ^0.24.1 |
| AI observability | Langfuse | (see `.env.local`) |
| Error tracking | Sentry Next.js | ^10.43.0 |
| RPA / EMR bridge | Playwright | ^1.40.0 |
| Validation | Zod | ^4.3.6 |
| Auth | Custom scrypt + HMAC sessions (12h TTL) | — |

## App Structure

```text
intelligenceBoard/
├── server.ts                  ← Custom HTTP server (Socket.IO + Next.js)
├── next.config.ts
├── prisma/
│   └── schema.prisma          ← Database schema
├── src/
│   ├── app/                   ← Next.js App Router (22 routes)
│   │   ├── api/               ← 90+ API route handlers
│   │   └── (pages)/           ← UI pages
│   ├── components/            ← React components
│   └── lib/                   ← Business logic, intelligence, integrations
│       ├── intelligence/      ← CDSS, trajectory, NEWS2, classifiers
│       ├── calculators/       ← Medical calculators (BMI, eGFR, qSOFA, etc.)
│       ├── audit/             ← Immutable hash-chain audit log
│       └── server/            ← Server-only utilities
├── scripts/                   ← Security + CDSS test scripts
├── public/                    ← Static assets
└── .github/workflows/
    └── ci.yml                 ← Dashboard Security Baseline (pnpm CI)
```

## Key Runtime Systems

| System | Path | Notes |
|--------|------|-------|
| Custom dev server | `server.ts` | HTTP server created before `next()` — required for Socket.IO HMR |
| CDSS engine | `src/lib/intelligence/` | Clinical decision support, NEWS2, trajectory |
| EMR bridge | `src/lib/server/emr-session-storage.ts` | Playwright RPA → ePuskesmas |
| AI observability | `src/lib/intelligence/langfuse.config.ts` | Per-inference Gemini tracing |
| Sentry scrubber | `src/lib/intelligence/sentry.config.ts` | PHI stripped before send |
| Audit hash chain | `src/lib/audit/screening-immutable-hash.ts` | Tamper-evident log |
| Socket.IO bridges | `server.ts` | 4 namespaces: intelligence, EMR, telemedicine, NOTAM |

## Environment

Required variables (see `.env.example`):
- `DATABASE_URL` — Neon PostgreSQL connection string
- `NEXTAUTH_SECRET` — 32-byte hex session secret
- `GEMINI_API_KEY` — Google Gemini inference
- `LIVEKIT_API_KEY` / `LIVEKIT_API_SECRET` / `LIVEKIT_URL` — telemedicine
- `LANGFUSE_*` — AI observability (optional)
- `SENTRY_DSN` — error tracking (optional)

## CI/CD

- Repo: `github.com/Claudesy/intelligenceBoard`
- Workflow: `.github/workflows/ci.yml` — Dashboard Security Baseline
- Install: `pnpm run ci:install` (frozen lockfile + prisma generate)
- Security baseline: `pnpm run security:baseline` — needs `DATABASE_URL`
- Local dev: `pnpm dev` from app root (port 7000)

## Hard Constraints

- PHI/PII — absolute prohibition in logs, commits, fixtures, test data
- `.env.local` — never commit; never print secrets to stdout
- `terraform apply` — Chief only
- Database migrations — Class C task, requires explicit GO
- JET Protocol J5 hard gate — no execution before "GO" for Class C tasks
- Session logs — `.agent/sessions/YYYY-MM-DD.md` must be updated every session
- Max 1 new file per task — fix same artifact, do not create replacements
- Push to remote — only with explicit Chief approval
