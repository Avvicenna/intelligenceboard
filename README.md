# DESIGN.md — Puskesmas Intelligence Dashboard

> Deep-dive system design, UX guidelines, and architecture rationale.
> Companion document to [README.md](./README.md).

---

## Table of Contents

- [System Architecture Diagrams](#system-architecture-diagrams)
- [Entity-Relationship Diagram](#entity-relationship-diagram)
- [Critical User Flow Diagrams](#critical-user-flow-diagrams)
- [UX & Visual Design Guidelines](#ux--visual-design-guidelines)
- [Component Design System](#component-design-system)
- [Primary Screen Wireframes](#primary-screen-wireframes)
- [Tech Stack Rationale](#tech-stack-rationale)
- [Security Architecture](#security-architecture)

---

## System Architecture Diagrams

### Full System Context Diagram

```mermaid
C4Context
    title Puskesmas Intelligence Dashboard — System Context

    Person(doctor, "Doctor / GP", "Diagnoses patients, uses CDSS, Audrey")
    Person(midwife, "Midwife / Bidan", "ANC tracking, ICD coding")
    Person(admin, "Clinical Admin", "Reports, scheduling, billing")
    Person(patient, "Patient", "Joins telemedicine via link")

    System(pid, "Puskesmas Intelligence Dashboard", "Clinical operations platform")

    System_Ext(epusk, "ePuskesmas", "National EMR (Kemenkes)")
    System_Ext(satu, "Satu Sehat", "MOH Health Data Platform")
    System_Ext(bpjs, "P-Care BPJS", "National Insurance Platform")
    System_Ext(gemini, "Google Gemini 2.5", "AI / LLM Service")
    System_Ext(turn, "STUN/TURN Server", "WebRTC NAT traversal")
    System_Ext(sms, "SMS/WhatsApp Gateway", "Patient notifications")

    Rel(doctor, pid, "Uses via browser")
    Rel(midwife, pid, "Uses via browser")
    Rel(admin, pid, "Uses via browser")
    Rel(patient, pid, "Joins telemedicine via one-time URL")

    Rel(pid, epusk, "RPA via Playwright")
    Rel(pid, satu, "Redirect / future API")
    Rel(pid, bpjs, "Redirect / future API")
    Rel(pid, gemini, "REST API (Audrey, CDSS, SOAP)")
    Rel(pid, turn, "WebRTC ICE")
    Rel(pid, sms, "Notification dispatch")
```

### Component Diagram

```mermaid
graph TB
    subgraph "Presentation Layer"
        NavBar["AppNav (Sidebar)"]
        AuthGate["CrewAccessGate"]
        ThemeProv["ThemeProvider"]
        Pages["Page Components\n(Dashboard, EMR, ICD, Report, Voice, Chat, Telemedicine)"]
    end

    subgraph "API Layer (Next.js Route Handlers)"
        AuthAPI["Auth API\n/api/auth/*"]
        EMRAPI["EMR API\n/api/emr/*"]
        ICDAPI["ICD API\n/api/icdx/*"]
        ReportAPI["Report API\n/api/report/*"]
        VoiceAPI["Voice API\n/api/voice/*"]
        CDSSAPI["CDSS API\n/api/cdss/*"]
        TeleAPI["Telemedicine API\n/api/telemedicine/*"]
    end

    subgraph "Service Layer (lib/)"
        AuthLib["crew-access.ts"]
        EMRLib["EMR Engine\n(Playwright orchestrator)"]
        LB1Lib["LB1 Engine\n(transform, validate, write)"]
        ICDLib["ICD Database\n(in-memory + DB)"]
        TeleLib["Telemedicine Lib\n(WebRTC, signaling, SOAP)"]
    end

    subgraph "Infrastructure"
        SocketServer["Socket.IO Server\n(server.ts)"]
        DB[("PostgreSQL")]
        FileStore["File Storage\n(reports, recordings)"]
        RuntimeFiles["Runtime Config\n(JSON, YAML)"]
    end

    Pages --> AuthAPI
    Pages --> EMRAPI
    Pages --> ICDAPI
    Pages --> ReportAPI
    Pages --> VoiceAPI
    Pages --> CDSSAPI
    Pages --> TeleAPI
    Pages <-->|WebSocket| SocketServer

    AuthAPI --> AuthLib
    EMRAPI --> EMRLib
    ReportAPI --> LB1Lib
    ICDAPI --> ICDLib
    TeleAPI --> TeleLib

    EMRLib --> SocketServer
    LB1Lib --> FileStore
    TeleLib --> SocketServer

    AuthLib --> DB
    EMRLib --> DB
    LB1Lib --> DB
    ICDLib --> DB
    TeleLib --> DB
    AuthLib --> RuntimeFiles
```

---

## Entity-Relationship Diagram

```mermaid
erDiagram
    STAFF {
        uuid id PK
        varchar username UK
        varchar display_name
        varchar role
        varchar department
        boolean is_active
        timestamptz created_at
    }

    PATIENT {
        uuid id PK
        varchar mrn UK
        varchar full_name
        date date_of_birth
        varchar sex
        varchar nik
        varchar phone
        varchar address
        uuid assigned_midwife_id FK
        timestamptz created_at
    }

    VISIT {
        uuid id PK
        uuid patient_id FK
        uuid doctor_id FK
        date visit_date
        text chief_complaint
        text anamnesis
        jsonb vitals
        text soap_note
        varchar status
        timestamptz created_at
    }

    DIAGNOSIS {
        uuid id PK
        uuid visit_id FK
        varchar icd_code
        varchar icd_description
        boolean is_primary
    }

    PRESCRIPTION {
        uuid id PK
        uuid visit_id FK
        varchar drug_name
        varchar dose
        varchar frequency
        integer duration_days
        text instructions
    }

    ANC_RECORD {
        uuid id PK
        uuid patient_id FK
        uuid midwife_id FK
        integer gestational_age_weeks
        date visit_date
        varchar gravida
        varchar para
        numeric weight_kg
        varchar bp_systolic
        varchar bp_diastolic
        varchar fetal_heart_rate
        varchar fetal_position
        varchar fundal_height_cm
        text notes
        varchar risk_level
    }

    EMR_TRANSFER_RUN {
        uuid id PK
        uuid visit_id FK
        varchar initiated_by FK
        varchar status
        jsonb payload
        text error_message
        timestamptz started_at
        timestamptz completed_at
    }

    TELEMEDICINE_SESSION {
        uuid id PK
        varchar doctor_id FK
        varchar patient_token UK
        varchar patient_name
        varchar status
        timestamptz scheduled_at
        timestamptz started_at
        timestamptz ended_at
        text recording_path
        text soap_note
        boolean emr_saved
    }

    CHAT_MESSAGE {
        uuid id PK
        uuid room_id FK
        varchar sender FK
        text content
        timestamptz created_at
    }

    STAFF ||--o{ VISIT : "conducts"
    STAFF ||--o{ ANC_RECORD : "records"
    STAFF ||--o{ TELEMEDICINE_SESSION : "hosts"
    PATIENT ||--o{ VISIT : "has"
    PATIENT ||--o{ ANC_RECORD : "has"
    VISIT ||--o{ DIAGNOSIS : "has"
    VISIT ||--o{ PRESCRIPTION : "has"
    VISIT ||--o| EMR_TRANSFER_RUN : "triggers"
```

---

## Critical User Flow Diagrams

### Patient Registration & First ANC Visit

```mermaid
sequenceDiagram
    actor Midwife
    participant Dashboard
    participant API as /api/pasien
    participant DB as PostgreSQL
    participant EMR as ePuskesmas

    Midwife->>Dashboard: Navigate to Pasien → New Patient
    Midwife->>Dashboard: Fill: Name, NIK, DOB, Phone, Address
    Dashboard->>API: POST /api/pasien {patientData}
    API->>DB: INSERT INTO patient
    DB-->>API: {patientId, mrn}
    API-->>Dashboard: 201 Created {patient}
    Dashboard-->>Midwife: Show patient profile

    Midwife->>Dashboard: Add ANC Record
    Midwife->>Dashboard: Fill: GA weeks, BP, weight, FHR, fundal height
    Dashboard->>API: POST /api/anc {ancData, patientId}
    API->>DB: INSERT INTO anc_record
    API->>DB: Calculate risk level (rule engine)
    DB-->>API: {ancId, riskLevel}
    API-->>Dashboard: 201 Created
    Dashboard-->>Midwife: Show ANC record + risk badge

    alt High-risk flagged
        Dashboard-->>Midwife: ⚠️ High-risk alert banner
        Dashboard-->>Midwife: Recommend specialist referral
    end
```

### Emergency Alert Flow (Future Feature)

```mermaid
flowchart TD
    A[Midwife identifies emergency\ne.g., PPH, eclampsia] --> B[Press Emergency Alert button]
    B --> C[Dashboard sends alert via Socket.IO]
    C --> D[All online staff receive push notification]
    C --> E[SMS sent to doctor on-call]
    D --> F[Doctor acknowledges alert]
    F --> G[Incident room created in ACARS]
    G --> H[Team coordinates via ACARS chat]
    H --> I{Needs referral?}
    I -- Yes --> J[Initiate referral workflow\nSend to RS PONED]
    I -- No --> K[Manage at Puskesmas\nDocument in EMR]
```

---

## UX & Visual Design Guidelines

### Design Principles

1. **Clinical-first clarity** — Information hierarchy prioritizes patient safety data (alerts, vitals, risk flags) over administrative data
2. **Mobile-first, desktop-optimized** — Midwives and nurses frequently use tablets and mobile phones at the bedside
3. **Offline resilience** (future) — Core read functions should work offline with service worker caching
4. **Accessibility (WCAG 2.1 AA)** — Minimum contrast ratio 4.5:1 for body text; 3:1 for UI components; keyboard-navigable; screen-reader compatible

### Color Palette

| Role | Light Mode | Dark Mode | Usage |
|---|---|---|---|
| Primary | `#0066CC` | `#4DA3FF` | CTAs, links, active states |
| Success | `#16A34A` | `#4ADE80` | Completed transfers, normal vitals |
| Warning | `#D97706` | `#FCD34D` | Medium risk, pending actions |
| Danger | `#DC2626` | `#F87171` | High risk, errors, emergencies |
| Neutral 900 | `#111827` | `#F9FAFB` | Primary text |
| Neutral 50 | `#F9FAFB` | `#111827` | Page background |
| Surface | `#FFFFFF` | `#1F2937` | Card backgrounds |

> **Note:** These are standard Tailwind CSS color tokens. The dashboard supports both light and dark themes via `ThemeProvider.tsx` and CSS custom properties in `globals.css`.

### Typography

| Scale | Font | Size | Weight | Use |
|---|---|---|---|---|
| Display | Geist Sans | 32px | 700 | Page titles |
| Heading 1 | Geist Sans | 24px | 600 | Section headers |
| Heading 2 | Geist Sans | 18px | 600 | Card headers |
| Body | Geist Sans | 14px | 400 | Body text, table content |
| Caption | Geist Sans | 12px | 400 | Metadata, timestamps |
| Mono | Geist Mono | 13px | 400 | Code, ICD codes, MRN values |

### Spacing System

Uses 4px base grid. Key spacing tokens: `4, 8, 12, 16, 24, 32, 48, 64px`

### Design Library Recommendation

**Recommendation: Tailwind CSS + shadcn/ui**

| Library | Pros | Cons |
|---|---|---|
| **Tailwind CSS + shadcn/ui** ✅ | Utility-first, highly customizable, accessible by default, tree-shakeable | Learning curve for non-Tailwind devs |
| Material UI (MUI) | Rich component library, well-documented | Heavy bundle, opinionated styling |
| Fluent UI (Microsoft) | Accessible, enterprise-grade | React-only, Microsoft aesthetic |
| Ant Design | Large component set, good for admin UIs | Very opinionated, large bundle |

**Rationale:** `shadcn/ui` builds on Radix UI primitives (fully accessible) with Tailwind styling — giving full control without sacrificing accessibility compliance. It aligns with the existing Geist font system and the project's TypeScript-first approach.

---

## Component Design System

### Key Components

#### `<PatientCard />`
Displays a compact patient summary: MRN, name, age, risk badge, last visit date.

```tsx
<PatientCard
  mrn="PKM-2026-00123"
  name="Ny. Sari Dewi"
  age={28}
  gestationalAge="36 weeks"
  riskLevel="high"       // 'low' | 'medium' | 'high'
  lastVisit="2026-04-14"
/>
```

#### `<VitalsBadge />`
Compact vitals display with color-coded normal/abnormal indicators.

```tsx
<VitalsBadge
  bp={{ systolic: 150, diastolic: 100 }}   // 🔴 Hypertensive
  heartRate={98
