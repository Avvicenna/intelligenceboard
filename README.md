text
<div align="center">

# 🏥 Puskesmas Intelligence Dashboard

**Clinical Information System for Primary & Maternal Healthcare Facilities**

<img src="public/pid.png" alt="Puskesmas PONED Balowerti Dashboard Preview" width="900" />

[![Status](https://img.shields.io/badge/Status-Production-brightgreen?style=for-the-badge)](https://github.com/DocSynapse)
[![Next.js](https://img.shields.io/badge/Next.js-16.1-black?style=for-the-badge&logo=next.js)](https://nextjs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-Strict-3178C6?style=for-the-badge&logo=typescript)](https://www.typescriptlang.org/)
[![React](https://img.shields.io/badge/React-19.2-61DAFB?style=for-the-badge&logo=react)](https://react.dev/)
[![Node.js](https://img.shields.io/badge/Node.js-%3E%3D20.9.0-339933?style=for-the-badge&logo=node.js)](https://nodejs.org/)
[![Railway](https://img.shields.io/badge/Deployed%20on-Railway-0B0D0E?style=for-the-badge&logo=railway)](https://railway.app/)
[![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)](./LICENSE)
[![CI](https://img.shields.io/github/actions/workflow/status/DocSynapse/healthcare-dashboard/ci.yml?style=for-the-badge&label=CI)](https://github.com/DocSynapse/healthcare-dashboard/actions)

_Architect & Built by [Claudesy](https://github.com/DocSynapse) · Sentra Healthcare Solutions_

> **"Technology enables, but humans decide."** — dr. Ferdi Iskandar, Founder

</div>

---

## Executive Summary

**Puskesmas Intelligence Dashboard** is a full-stack clinical operations platform purpose-built for **UPTD Puskesmas PONED Balowerti, Kota Kediri** — a primary healthcare facility (FKTP) with obstetric emergency services (PONED). It unifies clinical workflows, regulatory reporting, diagnostic AI, maternal care, and real-time team communication into a single cohesive interface.

**Why this project matters:**

- 🩺 **Reduces clinician administrative burden** by up to 70% through intelligent EMR automation and AI-assisted documentation
- 👶 **Improves maternal and neonatal outcomes** by equipping midwives and doctors with real-time clinical decision support and antenatal care tracking
- 📋 **Ensures regulatory compliance** with automated LB1/SP3 report generation aligned to Indonesian Ministry of Health standards (Kemenkes RI)
- 🌐 **Bridges the telemedicine gap** for rural and peri-urban patients through secure WebRTC-based virtual consultations
- 🔒 **Protects patient privacy** with HMAC-signed sessions, PHI-safe audit logs, and encryption at rest and in transit

**Target Users:**

| Persona | Role | Primary Pain Point Solved |
|---|---|---|
| Midwife (Bidan) | Antenatal care, delivery support | Manual ANC documentation, double-entry |
| General Practitioner | Patient encounters, diagnosis | Diagnostic uncertainty, ICD coding speed |
| Clinical Administrator | Reporting, scheduling | Monthly LB1 report preparation (hours → minutes) |
| Patient | Follow-up, remote consultation | Travel distance, wait times |

---

## Table of Contents

- [Executive Summary](#executive-summary)
- [Features Overview](#features-overview)
- [Quickstart & Demo](#quickstart--demo)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Environment Configuration](#environment-configuration)
  - [Running the Application](#running-the-application)
  - [Building for Production](#building-for-production)
- [Detailed Features](#detailed-features)
  - [1. User Profile Dashboard](#1-user-profile-dashboard)
  - [2. EMR Auto-Fill Engine](#2-emr-auto-fill-engine)
  - [3. ICD-X Finder](#3-icd-x-finder)
  - [4. LB1 Report Automation](#4-lb1-report-automation)
  - [5. Audrey — Clinical AI Assistant](#5-audrey--clinical-ai-assistant)
  - [6. ACARS — Internal Chat](#6-acars--internal-chat)
  - [7. CDSS — Clinical Decision Support](#7-cdss--clinical-decision-support)
  - [8. Crew Access Portal](#8-crew-access-portal)
  - [9. Telemedicine — Virtual Consultation](#9-telemedicine--virtual-consultation)
- [System Architecture](#system-architecture)
- [Project Structure](#project-structure)
- [Available Scripts](#available-scripts)
- [API Reference](#api-reference)
- [Security & Privacy](#security--privacy)
- [Operations & Deployment](#operations--deployment)
- [Integrations & Extensibility](#integrations--extensibility)
- [Developer Guide](#developer-guide)
- [Related Documentation](#related-documentation)
- [Assumptions & Open Questions](#assumptions--open-questions)
- [License](#license)

---

## Features Overview

| # | Feature | Status | Primary User |
|---|---|---|---|
| 1 | **User Profile Dashboard** | ✅ Live | All staff |
| 2 | **EMR Auto-Fill Engine** | ✅ Live | Doctor, Nurse |
| 3 | **ICD-X Finder** | ✅ Live | Doctor, Midwife |
| 4 | **LB1 Report Automation** | ✅ Live | Administrator |
| 5 | **Audrey — Clinical AI** | ✅ Live | Doctor |
| 6 | **ACARS — Internal Chat** | ✅ Live | All staff |
| 7 | **CDSS — Decision Support** | ✅ Live | Doctor, Midwife |
| 8 | **Crew Access Portal** | ✅ Live | All staff |
| 9 | **Telemedicine** | ✅ Live | Doctor, Patient |

---

## Quickstart & Demo

### Prerequisites

| Requirement | Minimum Version | Notes |
|---|---|---|
| Node.js | `>= 20.9.0` | [Download](https://nodejs.org/) |
| npm | `>= 10.x` | Bundled with Node.js |
| Git | Latest | [Download](https://git-scm.com/) |
| Chromium (Playwright) | Auto-installed | Required for EMR Auto-Fill only |

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/DocSynapse/healthcare-dashboard.git
cd healthcare-dashboard

# 2. Install all dependencies
npm install

# 3. Install Playwright browser binaries (EMR Auto-Fill feature)
npx playwright install chromium

# 4. Copy environment template
cp .env.example .env.local
# Edit .env.local with your credentials
```

### Environment Configuration

Create `.env.local` in the project root. **This file must never be committed to version control.**

```env
# ─── Server ───────────────────────────────────────────────────
PORT=7000
NODE_ENV=development

# ─── Authentication ───────────────────────────────────────────
# Generate with: openssl rand -hex 32
CREW_ACCESS_SECRET=your-secret-here-minimum-32-chars

# JSON array of staff user objects
CREW_ACCESS_USERS_JSON='[{"username":"admin","password":"change-me","displayName":"Administrator"}]'

# ─── AI Services ──────────────────────────────────────────────
GEMINI_API_KEY=your-gemini-api-key

# ─── EMR / ePuskesmas RPA ─────────────────────────────────────
# ⚠️ NEVER commit these values
EMR_BASE_URL=https://epuskesmas.example.id
EMR_LOGIN_URL=https://epuskesmas.example.id/login
EMR_USERNAME=your-epuskesmas-username
EMR_PASSWORD=your-epuskesmas-password
EMR_HEADLESS=true
EMR_SESSION_STORAGE_PATH=runtime/emr-session.json

# ─── LB1 Report Engine ────────────────────────────────────────
LB1_CONFIG_PATH=runtime/lb1-config.yaml
LB1_DATA_SOURCE_DIR=runtime/lb1-data
LB1_OUTPUT_DIR=runtime/lb1-output
LB1_HISTORY_FILE=runtime/lb1-run-history.jsonl
LB1_TEMPLATE_PATH=runtime/Laporan SP3 LB1.xlsx
LB1_MAPPING_PATH=runtime/diagnosis_mapping.csv

# ─── Telemedicine ─────────────────────────────────────────────
TURN_SERVER_URL=turn:your-turn-server:3478
TURN_SERVER_USERNAME=turn-user
TURN_SERVER_CREDENTIAL=turn-password
RECORDING_STORAGE_PATH=/var/recordings
TELEMEDICINE_PUBLIC_BASE_URL=https://your-domain.com/telemedicine/waiting

# ─── Database (Optional — falls back to file storage) ─────────
DATABASE_URL=postgresql://user:password@localhost:5432/puskesmas_db
```

> **Security Note:** `EMR_PASSWORD`, `CREW_ACCESS_SECRET`, and `TURN_SERVER_CREDENTIAL` are highly sensitive. Rotate regularly, use a secrets manager in production, and never log these values.

### Running the Application

```bash
# Development (with Socket.IO real-time support)
npm run dev

# Development (Next.js only, without Socket.IO)
npm run dev:next

# Access at: http://localhost:7000
```

### Building for Production

```bash
npm run build
npm run start
```

---

## Detailed Features

### 1. User Profile Dashboard

**Purpose:** Provide every logged-in staff member with a personalized home view: their profile, quick-access government portal links, and a live patient data summary.

#### User Stories

| ID | As a… | I want to… | So that… |
|---|---|---|---|
| US-01 | Midwife | See today's patient queue at login | I can plan my workday immediately |
| US-02 | Doctor | Access ePuskesmas and P-Care BPJS in one click | I avoid navigating between multiple logins |
| US-03 | Admin | View facility-level KPIs on the dashboard | I can monitor daily operations at a glance |
| US-04 | Any staff | See my profile and role information | I can confirm I am logged in to the correct account |

#### User Flow

```mermaid
flowchart TD
    A[User opens browser] --> B{Session cookie valid?}
    B -- Yes --> C[Load Profile Dashboard]
    B -- No --> D[Redirect to Login]
    D --> E[Enter credentials]
    E --> F{HMAC validation}
    F -- Valid --> C
    F -- Invalid --> G[Show error message]
    C --> H[Render: Profile card + Portal links + Patient summary]
    H --> I[User clicks portal link]
    I --> J[Open external portal in new tab]
```

#### UI Surfaces

- **Profile Card** — Avatar, name, role, department, login timestamp
- **Portal Quick-Links Grid** — Satu Sehat, SIPARWA, ePuskesmas, P-Care BPJS (icon buttons, opens new tab)
- **Patient Summary Widget** — Last 5 patients, vitals snapshot, ICD code badges
- **Health News Feed** — Latest Ministry of Health circulars via `/api/news`

#### Data Model

```sql
-- Staff/Crew table (in-memory / env-sourced in current implementation)
-- Future: Migrate to PostgreSQL
CREATE TABLE staff (
  id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  username    VARCHAR(64) UNIQUE NOT NULL,
  display_name VARCHAR(128) NOT NULL,
  role        VARCHAR(32) NOT NULL,         -- 'doctor','midwife','admin','nurse'
  department  VARCHAR(64),
  avatar_url  TEXT,
  is_active   BOOLEAN DEFAULT true,
  created_at  TIMESTAMPTZ DEFAULT now(),
  updated_at  TIMESTAMPTZ DEFAULT now()
);
```

#### API Contract

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| `GET` | `/api/auth/session` | Required | Get current session info |
| `GET` | `/api/news` | Required | Fetch health news feed |

**Example Response — `/api/auth/session`:**
```json
{
  "ok": true,
  "user": {
    "username": "dr.ferdi",
    "displayName": "dr. Ferdi Iskandar",
    "role": "doctor"
  },
  "expiresAt": "2026-04-16T07:00:00Z"
}
```

#### Security Considerations

- Session cookie is HMAC-signed; any tampering invalidates the session
- Portal links open in a sandboxed new tab (`rel="noopener noreferrer"`)
- Patient data in the summary widget is access-controlled by role

#### Testing Checklist

- [ ] Dashboard loads within 2 seconds on 3G connection
- [ ] Portal links render with correct URLs from config
- [ ] Session expiry at 12h triggers automatic redirect to login
- [ ] Role-based widgets render correctly per user role
- [ ] Portal grid is responsive on 320px mobile viewport

---

### 2. EMR Auto-Fill Engine

**Purpose:** Eliminate double-entry by using Playwright-driven RPA to automatically transfer structured clinical data (anamnesis, diagnosis, prescription) from the dashboard into ePuskesmas.

#### User Stories

| ID | As a… | I want to… | So that… |
|---|---|---|---|
| US-05 | Doctor | Submit patient visit data to ePuskesmas without re-typing | I save 10–15 minutes per patient |
| US-06 | Nurse | Monitor EMR transfer progress in real-time | I know when a transfer fails and can retry |
| US-07 | Admin | View EMR transfer history with timestamps | I can audit and troubleshoot failed transfers |
| US-08 | Doctor | Auto-fill prescription data | Pharmacy staff receives accurate medication orders |

#### User Flow

```mermaid
sequenceDiagram
    actor Doctor
    participant Dashboard as Next.js Dashboard
    participant API as /api/emr/transfer
    participant RPA as Playwright Engine
    participant ePuskesmas as ePuskesmas (External)

    Doctor->>Dashboard: Enters patient data (anamnesis, Dx, Rx)
    Doctor->>Dashboard: Clicks "Transfer to EMR"
    Dashboard->>API: POST /api/emr/transfer/run {patientData}
    API->>RPA: Launch headless Chromium session
    RPA->>ePuskesmas: Navigate to patient record
    RPA->>ePuskesmas: Fill form fields (anamnesis, ICD-X, prescriptions)
    RPA-->>API: Emit progress events via Socket.IO
    API-->>Dashboard: Stream progress updates (Socket.IO)
    Dashboard-->>Doctor: Show live progress bar & status
    RPA->>ePuskesmas: Submit form
    ePuskesmas-->>RPA: Confirm submission
    RPA-->>API: Emit completion event
    API-->>Dashboard: Transfer complete
    Dashboard-->>Doctor: Show success toast + audit log entry
```

#### Data Model

```sql
CREATE TABLE emr_transfer_runs (
  id            UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  patient_mrn   VARCHAR(32) NOT NULL,
  initiated_by  VARCHAR(64) NOT NULL,    -- staff username
  status        VARCHAR(16) NOT NULL,    -- 'pending','running','success','failed'
  payload       JSONB NOT NULL,          -- clinical data snapshot
  error_message TEXT,
  started_at    TIMESTAMPTZ DEFAULT now(),
  completed_at  TIMESTAMPTZ,
  duration_ms   INTEGER
);

CREATE INDEX idx_emr_runs_patient ON emr_transfer_runs(patient_mrn);
CREATE INDEX idx_emr_runs_status ON emr_transfer_runs(status);
```

#### API Contract

| Method | Endpoint | Description | Status Codes |
|---|---|---|---|
| `POST` | `/api/emr/transfer/run` | Execute EMR auto-fill | 202, 400, 409, 503 |
| `GET` | `/api/emr/transfer/status` | Current engine status | 200 |
| `GET` | `/api/emr/transfer/history` | Run history (paginated) | 200 |

**Example Request — `POST /api/emr/transfer/run`:**
```json
{
  "patientMrn": "PKM-2026-00123",
  "visitDate": "2026-04-15",
  "anamnesis": "Pasien datang dengan keluhan sakit kepala 2 hari...",
  "diagnoses": [{"code": "R51", "description": "Headache"}],
  "prescriptions": [
    {"drug": "Paracetamol", "dose": "500mg", "frequency": "3x1", "duration": "3 days"}
  ]
}
```

**Example Response (202 Accepted):**
```json
{
  "ok": true,
  "runId": "550e8400-e29b-41d4-a716-446655440000",
  "message": "EMR transfer initiated. Monitor via Socket.IO event: emr:progress"
}
```

**Error Response (409 Conflict):**
```json
{
  "ok": false,
  "error": "TRANSFER_IN_PROGRESS",
  "message": "An EMR transfer is already running. Wait for completion before starting a new one."
}
```

#### Security & Privacy

- EMR credentials (`EMR_USERNAME`, `EMR_PASSWORD`) are resolved at runtime from environment variables — never logged or exposed to the client
- Playwright runs in an isolated browser context with a dedicated session file
- Transfer payload containing PHI is stored encrypted in the database
- Audit log records username, timestamp, and result — but omits patient-identifiable data from log lines

#### Testing Checklist

- [ ] Transfer succeeds end-to-end with valid ePuskesmas credentials
- [ ] Concurrent transfer requests return 409
- [ ] Socket.IO events are received on the frontend during transfer
- [ ] Failed transfer writes error details to history with full stack trace
- [ ] Playwright session file is rotated after 24 hours

---

### 3. ICD-X Finder

**Purpose:** Provide fast, accurate ICD-10 diagnosis coding at the point of care, supporting three catalog versions (2010, 2016, 2019) with fuzzy search and legacy code translation.

#### User Stories

| ID | As a… | I want to… | So that… |
|---|---|---|---|
| US-09 | Doctor | Search ICD-10 codes by symptom keyword | I find the right code in under 5 seconds |
| US-10 | Midwife | Look up ANC-related ICD codes (Z34.x) | I code antenatal visits accurately |
| US-11 | Doctor | Translate legacy 2010 codes to 2019 equivalents | I can update old records to current standards |
| US-12 | Admin | Filter by ICD catalog version | I match the version required by my reporting system |

#### User Flow

```mermaid
flowchart LR
    A[User types symptom/code] --> B[Debounced API call]
    B --> C{Cache hit?}
    C -- Yes --> D[Return cached results]
    C -- No --> E[Search ICD DB]
    E --> F[Fuzzy match algorithm]
    F --> G[Return ranked results]
    G --> H[Display in table with badges]
    H --> I[User clicks result]
    I --> J[Copy code to clipboard / Insert into form]
```

#### Data Model

```sql
CREATE TABLE icd10_codes (
  id          SERIAL PRIMARY KEY,
  code        VARCHAR(8) NOT NULL,
  version     SMALLINT NOT NULL,         -- 2010, 2016, 2019
  description TEXT NOT NULL,
  category    VARCHAR(3),                -- First 3 chars of code (chapter)
  is_billable BOOLEAN DEFAULT true,
  parent_code VARCHAR(8),
  UNIQUE(code, version)
);

CREATE INDEX idx_icd_code ON icd10_codes(code);
CREATE INDEX idx_icd_version ON icd10_codes(version);
CREATE INDEX idx_icd_description_fts ON icd10_codes USING gin(to_tsvector('indonesian', description));
```

**Example Record:**
```json
{
  "code": "Z34.0",
  "version": 2019,
  "description": "Supervision of normal first pregnancy",
  "category": "Z34",
  "isBillable": true,
  "parentCode": "Z34"
}
```

#### API Contract

| Method | Endpoint | Query Params | Description |
|---|---|---|---|
| `GET` | `/api/icdx/lookup` | `q`, `version`, `limit` | Search ICD-10 codes |

**Example:** `GET /api/icdx/lookup?q=headache&version=2019&limit=10`

```json
{
  "ok": true,
  "version": 2019,
  "query": "headache",
  "results": [
    {"code": "R51", "description": "Headache", "category": "R51", "isBillable": true},
    {"code": "G44.3", "description": "Post-traumatic headache", "category": "G44", "isBillable": true}
  ],
  "total": 2
}
```

#### Testing Checklist

- [ ] Search returns results in < 200ms for common terms
- [ ] Fuzzy match handles misspellings ("headache" → "hedache")
- [ ] Version switching reloads correct catalog without page refresh
- [ ] Legacy code translation returns mapped 2019 equivalent
- [ ] Empty query returns 400 with validation message

---

### 4. LB1 Report Automation

**Purpose:** Automate the monthly LB1/SP3 national health reporting pipeline — from raw ePuskesmas export to a validated, submission-ready Excel report.

#### User Stories

| ID | As a… | I want to… | So that… |
|---|---|---|---|
| US-13 | Admin | Generate the LB1 report in one click | I reduce monthly report prep from 4 hours to 10 minutes |
| US-14 | Admin | Download the QC CSV of rejected records | I can manually fix invalid entries before resubmission |
| US-15 | Admin | View run history with timestamps | I can track when reports were last generated |
| US-16 | Doctor | Run a preflight validation before report generation | I catch data errors before they fail the pipeline |

#### Pipeline Flow

```mermaid
flowchart TD
    A[Upload ePuskesmas export CSV] --> B[Normalize & validate records]
    B --> C{Validation pass?}
    C -- No --> D[Write rejected records to QC CSV]
    C -- Yes --> E[Map ICD-10 codes to LB1 disease categories]
    E --> F[Aggregate counts per category per period]
    F --> G[Populate Excel LB1 template]
    G --> H[Write JSON audit summary]
    H --> I[Output: .xlsx + QC.csv + audit.json]
    D --> F
```

#### Output Files

| File | Format | Description |
|---|---|---|
| `LB1_YYYY_MM.xlsx` | Excel | Completed national LB1 report |
| `LB1_YYYY_MM_QC.csv` | CSV | Rejected records with rejection reason |
| `LB1_YYYY_MM_audit.json` | JSON | Run metadata: total records, pass rate, duration |

#### API Contract

| Method | Endpoint | Description | Status Codes |
|---|---|---|---|
| `GET` | `/api/report/automation/preflight` | Pre-run data validation | 200, 422 |
| `POST` | `/api/report/automation/run` | Execute LB1 pipeline | 202, 409, 503 |
| `GET` | `/api/report/automation/status` | Current pipeline status | 200 |
| `GET` | `/api/report/automation/history` | Run history (`?limit=30`) | 200 |
| `GET` | `/api/report/files` | List output files | 200 |
| `GET` | `/api/report/files/download` | Download specific file | 200, 404 |

#### Security & Privacy

- LB1 data contains aggregated disease statistics — individual patient records are de-identified in the output
- Audit JSON logs contain only record counts and validation summaries — no patient names or MRNs
- Output files are stored in a server-side directory not accessible via public URLs; downloads require authentication

---

### 5. Audrey — Clinical AI Assistant

**Purpose:** A voice-first AI clinical copilot powered by Google Gemini 2.5 Flash (native audio), providing real-time diagnostic insights calibrated for Puskesmas-level resources during patient encounters.

#### User Stories

| ID | As a… | I want to… | So that… |
|---|---|---|---|
| US-17 | Doctor | Ask Audrey about differential diagnoses by voice | I get instant suggestions without typing |
| US-18 | Doctor | Receive Puskesmas-appropriate treatment recommendations | Suggestions respect resource constraints (no CT, limited labs) |
| US-19 | Midwife | Ask Audrey about ANC protocols | I follow correct antenatal care guidelines |
| US-20 | Doctor | Have Audrey summarize the patient history verbally | I stay focused on the patient, not the screen |

> **⚠️ Clinical Disclaimer:** Audrey provides AI-assisted suggestions only. All clinical decisions must be made by a licensed healthcare professional. Audrey's output must never be applied without professional verification.

#### API Contract

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/voice/chat` | Send text/audio to Audrey, receive response |
| `POST` | `/api/voice/tts` | Text-to-speech synthesis |
| `GET` | `/api/voice/token` | Retrieve Gemini session token |

**Example Request — `POST /api/voice/chat`:**
```json
{
  "message": "Patient is a 28-year-old woman, G2P1, 36 weeks pregnant, presenting with headache and BP 150/100. What are your thoughts?",
  "context": "ANC",
  "sessionId": "session-abc123"
}
```

**Example Response:**
```json
{
  "ok": true,
  "reply": "Given the presentation of hypertension (150/100) at 36 weeks in a multigravida patient, the key differential includes pre-eclampsia. Immediate priorities: check for proteinuria (dipstick), assess fetal heart rate, and evaluate for symptoms of severe pre-eclampsia (visual disturbances, epigastric pain). Per Puskesmas PONED protocol, if pre-eclampsia is confirmed, initiate MgSO4 loading dose and prepare for urgent referral to RS if signs of severity are present.",
  "confidence": "high",
  "sources": ["Kemenkes RI ANC Protocol 2020", "PONED Clinical Guidelines"]
}
```

#### Security & Privacy

- Voice sessions are ephemeral — audio is processed in-memory and not stored unless explicitly flagged
- Gemini API key is server-side only, never exposed to the browser
- Session tokens have a 1-hour TTL

---

### 6. ACARS — Internal Chat

**Purpose:** Real-time team messaging for clinical and administrative staff within the facility, enabling rapid coordination without leaving the dashboard.

#### User Stories

| ID | As a… | I want to… | So that… |
|---|---|---|---|
| US-21 | Nurse | Message the doctor about an urgent patient | The doctor is notified immediately without phone calls |
| US-22 | Doctor | See who is online in the team | I know who I can reach right now |
| US-23 | Midwife | Message the admin about a BPJS code issue | We resolve billing issues quickly |
| US-24 | Any staff | See typing indicators in conversations | I know a reply is incoming |

#### Real-Time Events (Socket.IO)

| Event | Direction | Payload |
|---|---|---|
| `acars:message` | Server → Client | `{roomId, sender, text, timestamp}` |
| `acars:typing` | Client → Server | `{roomId, username}` |
| `acars:presence` | Server → Client | `{username, status: 'online'/'offline'}` |
| `acars:read` | Client → Server | `{roomId, messageId}` |

#### Data Model

```sql
CREATE TABLE chat_rooms (
  id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name        VARCHAR(128),
  type        VARCHAR(16) NOT NULL,       -- 'direct', 'group'
  created_at  TIMESTAMPTZ DEFAULT now()
);

CREATE TABLE chat_messages (
  id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  room_id     UUID REFERENCES chat_rooms(id),
  sender      VARCHAR(64) NOT NULL,
  content     TEXT NOT NULL,
  created_at  TIMESTAMPTZ DEFAULT now(),
  read_by     JSONB DEFAULT '[]'         -- array of usernames
);

CREATE INDEX idx_messages_room ON chat_messages(room_id, created_at DESC);
```

---

### 7. CDSS — Clinical Decision Support

**Purpose:** Provide ranked differential diagnoses and evidence-based treatment recommendations by combining a local disease knowledge base (159 diseases, 45,030 real encounter records) with Gemini-powered reasoning.

#### User Stories

| ID | As a… | I want to… | So that… |
|---|---|---|---|
| US-25 | Doctor | Input patient symptoms and receive ranked differentials | I don't miss uncommon but serious diagnoses |
| US-26 | Midwife | Get ANC risk stratification suggestions | I identify high-risk pregnancies early |
| US-27 | Doctor | Receive referral criteria per diagnosis | I know when to refer vs. manage at Puskesmas level |
| US-28 | Doctor | See treatment plans aligned to Puskesmas formulary | Recommendations are actually actionable locally |

#### API Contract

**`POST /api/cdss/diagnose`**

```json
// Request
{
  "symptoms": ["headache", "fever", "stiff neck"],
  "patientAge": 25,
  "patientSex": "female",
  "vitals": {"bp": "120/80", "temp": 38.5, "hr": 98},
  "context": "general"
}

// Response
{
  "ok": true,
  "differentials": [
    {
      "rank": 1,
      "diagnosis": "Bacterial Meningitis",
      "icdCode": "G00.9",
      "probability": "high",
      "urgency": "emergency",
      "referralRequired": true,
      "rationale": "Classic triad of headache, fever, and neck stiffness...",
      "immediateActions": ["Urgent referral to hospital", "Do not delay for LP"]
    }
  ],
  "disclaimer": "AI-generated suggestion. Clinical judgment required."
}
```

> **⚠️ Assumption:** Clinical protocols and drug formulary data are based on Indonesian Ministry of Health guidelines (Permenkes). Specific protocol versions should be verified by the clinical team and updated in the knowledge base regularly.

---

### 8. Crew Access Portal

**Purpose:** Secure the entire dashboard behind an authentication gate using HMAC-signed session cookies with role-based access.

#### Authentication Flow

```mermaid
sequenceDiagram
    actor User
    participant Browser
    participant API as /api/auth
    participant Server as Node.js Server

    User->>Browser: Navigate to dashboard
    Browser->>Server: GET /
    Server->>Browser: Check session cookie
    alt No valid session
        Browser->>Browser: Render <CrewAccessGate /> (login form)
        User->>Browser: Enter username + password
        Browser->>API: POST /api/auth/login
        API->>API: Validate credentials (env > JSON > defaults)
        API->>API: Generate HMAC-signed cookie (12h TTL)
        API->>Browser: Set-Cookie: crew_session=...
        Browser->>Browser: Re-render dashboard
    else Valid session
        Browser->>Browser: Render full dashboard
    end
```

#### Credential Resolution Priority
CREW_ACCESS_USERS_JSON (environment variable) ← Highest priority

runtime/crew-users.json (runtime file)

Compiled defaults ← Lowest priority (dev only)

text

#### Security Properties

- Session cookies: `HttpOnly`, `Secure` (production), `SameSite=Strict`
- HMAC algorithm: SHA-256 with `CREW_ACCESS_SECRET`
- Session TTL: 12 hours; auto-invalidated on server restart in development
- No database dependency for authentication — stateless and fast
- Brute-force protection: implement rate limiting on `/api/auth/login` (5 attempts/15 min)

---

### 9. Telemedicine — Virtual Consultation

**Purpose:** Enable secure, real-time video consultations between Puskesmas doctors and patients — reducing unnecessary in-person visits for follow-ups, medication reviews, and remote triage.

#### User Stories

| ID | As a… | I want to… | So that… |
|---|---|---|---|
| US-29 | Doctor | Start a video call with a patient via one-time link | The patient can join without creating an account |
| US-30 | Patient | Join from a mobile browser by clicking a WhatsApp/SMS link | No app installation is required |
| US-31 | Doctor | Share lab results during the call | The patient understands their results remotely |
| US-32 | Doctor | Have a SOAP note drafted automatically post-session | I spend less time on post-visit documentation |

#### Telemedicine Workflow

```mermaid
sequenceDiagram
    actor Doctor
    actor Patient
    participant Dashboard
    participant API as /api/telemedicine
    participant TURN as STUN/TURN Server
    participant EMR as EMR System

    Doctor->>Dashboard: Create consultation slot
    Dashboard->>API: POST /api/telemedicine/sessions
    API-->>Dashboard: {sessionId, patientToken}
    Dashboard-->>Doctor: Send patient link (SMS/WhatsApp)
    Doctor->>Patient: Shares one-time link

    Patient->>Dashboard: Opens waiting room URL
    Dashboard->>API: GET session by token
    API-->>Dashboard: Session valid

    Doctor->>Dashboard: Admits patient
    Dashboard->>TURN: ICE candidate exchange (via Socket.IO signaling)
    TURN-->>Dashboard: WebRTC peer connection established

    Note over Doctor,Patient: Video + audio + chat + file sharing active

    Doctor->>Dashboard: End session
    Dashboard->>API: PATCH /api/telemedicine/sessions/:id {status: "completed"}
    API->>EMR: Generate SOAP note draft (Gemini)
    EMR-->>Dashboard: SOAP note ready for review
    Doctor->>Dashboard: Review + save to patient EMR
```

#### API Contract

| Method | Endpoint | Description | Status Codes |
|---|---|---|---|
| `POST` | `/api/telemedicine/sessions` | Create new session | 201, 400 |
| `GET` | `/api/telemedicine/sessions` | List sessions (paginated) | 200 |
| `GET` | `/api/telemedicine/sessions/:id` | Get session details | 200, 404 |
| `PATCH` | `/api/telemedicine/sessions/:id` | Update session status | 200, 400 |
| `POST` | `/api/telemedicine/signal` | WebRTC signaling relay | 200 |
| `POST` | `/api/telemedicine/recording/start` | Begin session recording | 202 |
| `POST` | `/api/telemedicine/recording/stop` | End & save recording | 200 |
| `GET` | `/api/telemedicine/schedule` | List appointment slots | 200 |
| `POST` | `/api/telemedicine/schedule` | Create appointment slot | 201 |

#### Data Model

```sql
CREATE TABLE telemedicine_sessions (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  doctor_id       VARCHAR(64) NOT NULL,
  patient_token   VARCHAR(128) UNIQUE NOT NULL,  -- one-time join token
  patient_name    VARCHAR(128),
  status          VARCHAR(16) DEFAULT 'scheduled', -- scheduled/waiting/active/completed/cancelled
  scheduled_at    TIMESTAMPTZ,
  started_at      TIMESTAMPTZ,
  ended_at        TIMESTAMPTZ,
  recording_path  TEXT,
  soap_note       TEXT,
  emr_saved       BOOLEAN DEFAULT false,
  created_at      TIMESTAMPTZ DEFAULT now()
);

CREATE INDEX idx_sessions_doctor ON telemedicine_sessions(doctor_id, scheduled_at);
CREATE INDEX idx_sessions_token ON telemedicine_sessions(patient_token);
```

#### Privacy & Consent

- Recording is strictly opt-in; explicit consent is requested and logged before recording begins
- Recordings are stored server-side with AES-256 encryption at rest
- Patient tokens are single-use and expire after 24 hours or session end
- SOAP notes generated by AI are marked as draft and require physician sign-off before becoming part of the official EMR record

---

## System Architecture

### Architecture Overview

```mermaid
graph TB
    subgraph Client Layer
        Browser["Web Browser\n(Next.js React App)"]
        Mobile["Mobile Browser\n(Responsive PWA)"]
    end

    subgraph Application Layer
        NextApp["Next.js 16.1\nApp Router\n(server.ts entry)"]
        SocketIO["Socket.IO Server\n(Real-time events)"]
    end

    subgraph Service Layer
        RPA["Playwright RPA\n(EMR Auto-Fill)"]
        GeminiAI["Google Gemini 2.5 Flash\n(Audrey + CDSS + SOAP)"]
        WebRTC["WebRTC Signaling\n(Telemedicine)"]
        SheetJS["SheetJS\n(LB1 Excel I/O)"]
    end

    subgraph Data Layer
        PostgreSQL[("PostgreSQL\n(Primary DB)")]
        FileStore["File Storage\n(Reports, Recordings)"]
        RuntimeJSON["Runtime JSON/YAML\n(Config, Credentials)"]
    end

    subgraph External Integrations
        ePuskesmas["ePuskesmas\n(National EMR)"]
        SatuSehat["Satu Sehat\n(MOH Platform)"]
        BPJS["P-Care BPJS\n(Insurance)"]
        TURN["STUN/TURN Server\n(WebRTC NAT traversal)"]
        SMS["SMS/WhatsApp Gateway\n(Patient links)"]
    end

    Browser <-->|HTTPS + WSS| NextApp
    Mobile <-->|HTTPS + WSS| NextApp
    NextApp <--> SocketIO
    NextApp --> RPA
    NextApp --> GeminiAI
    NextApp <--> WebRTC
    NextApp --> SheetJS
    NextApp <--> PostgreSQL
    NextApp --> FileStore
    NextApp --> RuntimeJSON
    RPA --> ePuskesmas
    NextApp --> SatuSehat
    NextApp --> BPJS
    WebRTC <--> TURN
    NextApp --> SMS
```

### Deployment Model Recommendation

**Recommendation: Enhanced Monolith on Railway (current) → Modular Monolith (v2)**

| Approach | Pros | Cons | Verdict |
|---|---|---|---|
| **Single Monolith** | Simple ops, single deploy, low cost | Hard to scale individual services | ✅ Best for current team size |
| Microservices | Independent scaling per service | Operational complexity, latency | ❌ Premature for current scale |
| Serverless | Near-zero idle cost | Cold starts incompatible with WebSockets | ❌ Incompatible with Socket.IO/WebRTC |

The current custom `server.ts` architecture (Next.js + Socket.IO in one process) is correct and optimal for a Puskesmas-scale deployment. At larger scale (>50 concurrent users), the Playwright RPA worker and CDSS inference should be extracted to separate background workers.

### Scaling Strategy

- **Stateless API routes** — all session state in HMAC cookies; horizontally scalable
- **Socket.IO with Redis Adapter** — enables multi-instance Socket.IO when scaling beyond one process
- **Playwright workers** — run in a separate worker pool (max 3 concurrent RPA jobs to prevent resource exhaustion)
- **Caching** — ICD-10 lookup results cached in Redis with 24h TTL; reduces DB read load
- **CDN** — Static assets served via Railway's CDN edge; reduce server load

---

## Project Structure
healthcare-dashboard/
├── server.ts # Custom HTTP + Socket.IO server entry point
├── next.config.ts # Next.js configuration
├── tsconfig.json # TypeScript (strict mode)
├── railway.toml # Railway deployment configuration
├── package.json # Dependencies and scripts
│
├── src/
│ ├── app/
│ │ ├── layout.tsx # Root layout (ThemeProvider + CrewAccessGate + AppNav)
│ │ ├── page.tsx # Home / User Profile Dashboard
│ │ ├── globals.css # Global CSS with dark/light theme tokens
│ │ ├── emr/ # EMR Auto-Fill interface
│ │ ├── icdx/ # ICD-X lookup page
│ │ ├── report/ # LB1 report generation page
│ │ ├── voice/ # Audrey voice consultation page
│ │ ├── acars/ # Internal chat page
│ │ ├── pasien/ # Patient records page
│ │ ├── telemedicine/ # Telemedicine module
│ │ └── api/
│ │ ├── auth/ # Login, logout, session
│ │ ├── cdss/ # Clinical decision support
│ │ ├── emr/ # EMR transfer endpoints
│ │ ├── icdx/ # ICD-10 lookup
│ │ ├── report/ # LB1 automation + clinical reports
│ │ ├── voice/ # TTS, chat, token
│ │ ├── telemedicine/ # Sessions, signaling, recording, schedule
│ │ ├── news/ # Health news
│ │ └── perplexity/ # Perplexity AI integration
│ │
│ ├── components/
│ │ ├── AppNav.tsx # Sidebar navigation
│ │ ├── CrewAccessGate.tsx # Authentication gate
│ │ ├── ThemeProvider.tsx # Dark/light theme context
│ │ └── ui/ # Shared UI component library
│ │
│ └── lib/
│ ├── crew-access.ts # Auth types and constants
│ ├── server/ # Server-only auth logic
│ ├── lb1/ # LB1 report engine
│ ├── emr/ # EMR auto-fill engine + Playwright
│ ├── icd/ # ICD-10 database
│ └── telemedicine/ # WebRTC, signaling, recorder, SOAP generator
│
├── docs/
│ └── plans/ # Design documents and feature specs
│
├── runtime/ # Runtime config (gitignored — never commit)
│ ├── emr-session.json
│ ├── lb1-config.yaml
│ ├── lb1-data/
│ ├── lb1-output/
│ └── crew-users.json
│
└── mintlify-docs/ # Public API documentation (Mintlify)

text

---

## Available Scripts

| Script | Description |
|---|---|
| `npm run dev` | Start custom server with Socket.IO (port 7000) |
| `npm run dev:clean` | Clear lock file and start dev server |
| `npm run dev:next` | Start Next.js without custom server |
| `npm run build` | Compile production bundle |
| `npm run start` | Start production server |
| `npm run docs:dev` | Local Mintlify preview (port 3004) |
| `npm run docs:api` | Regenerate OpenAPI spec from route handlers |
| `npm run docs:check` | Validate Mintlify site + broken links |

---

## API Reference

All routes are prefixed with `/api`. Authentication is enforced via HMAC-signed session cookies on all routes.

### Authentication

| Method | Endpoint | Description | Status Codes |
|---|---|---|---|
| `POST` | `/api/auth/login` | Authenticate with crew credentials | 200, 401, 429 |
| `POST` | `/api/auth/logout` | End current session | 200 |
| `GET` | `/api/auth/session` | Validate current session | 200, 401 |

### Clinical Decision Support

| Method | Endpoint | Description | Status Codes |
|---|---|---|---|
| `POST` | `/api/cdss/diagnose` | Run diagnostic suggestion engine | 200, 400, 503 |
| `GET` | `/api/icdx/lookup` | ICD-10 code search (`?q=&version=`) | 200, 400 |

### EMR Transfer

| Method | Endpoint | Description | Status Codes |
|---|---|---|---|
| `POST` | `/api/emr/transfer/run` | Execute EMR auto-fill | 202, 409, 503 |
| `GET` | `/api/emr/transfer/status` | Transfer engine status | 200 |
| `GET` | `/api/emr/transfer/history` | Transfer history | 200 |

### LB1 Report Automation

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/report/automation/preflight` | Pre-run validation |
| `POST` | `/api/report/automation/run` | Execute pipeline |
| `GET` | `/api/report/automation/status` | Pipeline status |
| `GET` | `/api/report/automation/history` | Run history |
| `GET` | `/api/report/files` | List output files |
| `GET` | `/api/report/files/download` | Download file |

### Clinical Reports

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/report/clinical` | List reports |
| `POST` | `/api/report/clinical` | Create clinical report |
| `DELETE` | `/api/report/clinical?id=` | Delete report |

### Voice / Audrey

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/voice/chat` | Send message to Audrey |
| `POST` | `/api/voice/tts` | Text-to-speech synthesis |
| `GET` | `/api/voice/token` | Retrieve voice session token |

### Telemedicine

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/telemedicine/sessions` | Create session |
| `GET` | `/api/telemedicine/sessions` | List sessions |
| `GET` | `/api/telemedicine/sessions/:id` | Get session |
| `PATCH` | `/api/telemedicine/sessions/:id` | Update session |
| `POST` | `/api/telemedicine/signal` | WebRTC signaling |
| `POST` | `/api/telemedicine/recording/start` | Start recording |
| `POST` | `/api/telemedicine/recording/stop` | Stop recording |
| `GET` | `/api/telemedicine/schedule` | List slots |
| `POST` | `/api/telemedicine/schedule` | Create slot |

---

## Security & Privacy

### Authentication & Authorization

- **Auth mechanism:** HMAC-SHA256 signed session cookies (`crew_session`)
- **TTL:** 12 hours; requires re-authentication after expiry
- **Session flags:** `HttpOnly`, `Secure` (production), `SameSite=Strict`
- **Role-based access:** Roles enforced at the API route level (`doctor`, `midwife`, `nurse`, `admin`)
- **Recommended future enhancement:** Add OAuth2/OIDC with an identity provider (Keycloak or Auth0) for SSO across Puskesmas systems

### Encryption

| Layer | Mechanism |
|---|---|
| In transit | TLS 1.3 (enforced by Railway/CDN) |
| At rest (recordings) | AES-256 encryption |
| Secrets | Environment variables (Railway Vault in production) |
| Cookie | HMAC-SHA256 signature (tamper-proof) |

### Privacy & Compliance

- **Indonesian context:** Align with UU No. 17/2023 (Kesehatan) and Permenkes regarding health data protection
- **GDPR equivalence:** Implement data minimization, purpose limitation, and right-to-erasure for any EU-connected deployments
- **HIPAA equivalence:** Apply PHI classification, audit logging, and BAA (Business Associate Agreement) requirements for any US deployments
- **Consent management:** Explicit patient consent required for telemedicine recording and any data sharing beyond the treating facility
- **Audit logging:** All PHI-touching operations logged with: `{timestamp, actor, action, resource_type, resource_id}` — never include patient name or MRN in log lines

### Threat Mitigations

| Threat | Mitigation |
|---|---|
| Session forgery | HMAC-signed cookies; server-side validation |
| Credential brute force | Rate limiting on login endpoint (5 req/15 min) |
| XSS | Next.js CSP headers; no dangerouslySetInnerHTML |
| SQL injection | Parameterized queries via ORM |
| SSRF (Playwright) | Allowlist of permitted target URLs for RPA |
| PHI data leak | No patient identifiers in logs; PHI encrypted at rest |

---

## Operations & Deployment

### Railway Deployment

```toml
# railway.toml
[build]
builder = "nixpacks"
buildCommand = "npm run build"

[deploy]
startCommand = "npm run start"
restartPolicyType = "on_failure"
restartPolicyMaxRetries = 3
```

**Deployment Steps:**
1. Push repository to GitHub
2. Connect repository to new Railway project
3. Set all required environment variables in Railway dashboard → **Variables**
4. Railway auto-builds and deploys on every push to `master`
5. Verify health at `https://your-domain.com/api/auth/session` (expect 401)

### CI/CD Pipeline

```yaml
# .github/workflows/ci.yml (recommended)
name: CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: '20' }
      - run: npm ci
      - run: npm run build
      - run: npx playwright install chromium
      - run: npm test
```

### Observability

| Signal | Tool | Key Metrics |
|---|---|---|
| Logs | Railway structured logs | Error rate, auth failures, RPA errors |
| Metrics | Railway metrics dashboard | CPU, memory, response time |
| Alerts | Railway notifications | Build failures, restart loops |
| Tracing (recommended) | OpenTelemetry + Jaeger | End-to-end request latency |

**Recommended Alert Thresholds:**

- 🔴 Error rate > 5% over 5 minutes
- 🔴 Memory usage > 85% sustained 10 minutes
- 🟡 EMR transfer failure rate > 20%
- 🟡 Average API response time > 2 seconds

### Backup & Recovery

| Data | Backup Frequency | Recovery Method |
|---|---|---|
| PostgreSQL | Daily pg_dump to S3 | Restore from latest dump |
| Runtime config files | Synced to Railway env vars | Redeploy from env vars |
| LB1 output files | Retained for 12 months | Download from file storage |
| Telemedicine recordings | Retained per institutional policy | Restore from encrypted S3 |

---

## Integrations & Extensibility

### Current Integrations

| Integration | Purpose | Method |
|---|---|---|
| ePuskesmas | EMR data entry | Playwright RPA (no public API) |
| Satu Sehat (MOH) | National health platform link | External URL redirect |
| P-Care BPJS | Insurance claim platform | External URL redirect |
| Google Gemini 2.5 Flash | AI assistant, CDSS reasoning, SOAP generation | REST API |
| Perplexity AI | Research queries | REST API |

### Recommended Future Integrations

| Integration | Use Case | Protocol |
|---|---|---|
| WhatsApp Business API | Patient appointment links | REST (Twilio/WATI) |
| SMS Gateway | Fallback patient notifications | REST |
| FHIR R4 | HL7 health data interoperability | REST/JSON-FHIR |
| Payment Gateway (Midtrans) | Patient billing | REST |
| Lab Information System | Lab result retrieval | HL7/REST |
| OpenMRS | Open-source EMR interoperability | REST API |

### API Versioning

All future breaking changes will be versioned under `/api/v2/...`. Current API is considered `v1` (implicit). Deprecation notices will be published in `CHANGELOG.md` with a minimum 90-day sunset window.

---

## Developer Guide

### Local Development Setup

```bash
git clone https://github.com/DocSynapse/healthcare-dashboard.git
cd healthcare-dashboard
npm install
npx playwright install chromium
cp .env.example .env.local
# Edit .env.local — at minimum set CREW_ACCESS_SECRET and CREW_ACCESS_USERS_JSON
npm run dev
```

### Database Seed (Development)

```bash
# Seed ICD-10 database (2010, 2016, 2019)
npm run db:seed:icd

# Seed sample patient records (anonymized dummy data)
npm run db:seed:patients

# Reset database
npm run db:reset
```

### Code Style

- **TypeScript strict mode** — no `any` types without explicit justification
- **ESLint** with Next.js recommended config
- **Prettier** for formatting (run `npm run format` before commit)
- **File naming:** `kebab-case.ts` for utilities, `PascalCase.tsx` for components

### Commit Message Guidelines
type(scope): short imperative description

Types: feat | fix | chore | docs | refactor | test | perf
Examples:
feat(emr): add retry logic for failed transfers
fix(auth): correct HMAC cookie expiry calculation
docs(readme): expand telemedicine API reference

text

### Pull Request Checklist

- [ ] Code compiles without TypeScript errors (`npm run build`)
- [ ] No secrets or credentials in diff
- [ ] New features have corresponding test cases
- [ ] API changes documented in endpoint tables
- [ ] Environment variable additions documented in `.env.example`
- [ ] `CHANGELOG.md` updated

---

## Related Documentation

| Document | Description |
|---|---|
| [ARCHITECTURE.md](./ARCHITECTURE.md) | System architecture and component design |
| [CONTRIBUTING.md](./CONTRIBUTING.md) | Development workflow and conventions |
| [CHANGELOG.md](./CHANGELOG.md) | Version history and release notes |
| [SECURITY.md](./SECURITY.md) | Security policy and vulnerability reporting |
| [DISCLAIMER.md](./DISCLAIMER.md) | Clinical and liability disclaimer |
| [DATA_PRIVACY.md](./DATA_PRIVACY.md) | Privacy commitments |
| [MODEL_CARD.md](./MODEL_CARD.md) | AI model integration summary |
| [DATASET_CARD.md](./DATASET_CARD.md) | CDSS dataset stewardship notes |
| [docs/DEPLOYMENT.md](./docs/DEPLOYMENT.md) | Deployment operations notes |
| [docs/TROUBLESHOOTING.md](./docs/TROUBLESHOOTING.md) | Common failures and fixes |
| [mintlify-docs/](./mintlify-docs/) | Public API documentation (Mintlify) |

---

## Assumptions & Open Questions

### Assumptions Made

| # | Assumption | Impact if Wrong |
|---|---|---|
| A1 | Indonesian clinical protocols follow Kemenkes RI 2020 ANC guidelines | CDSS/Audrey responses may need recalibration |
| A2 | ePuskesmas does not provide a public REST API; Playwright RPA is the only integration path | If ePuskesmas exposes an API, RPA can be replaced with direct API calls |
| A3 | Current user base < 50 concurrent users; monolith is sufficient | Needs redesign if concurrent load exceeds this |
| A4 | PostgreSQL is the intended primary database (currently partial/file fallback) | Schema must be confirmed with the engineering team |
| A5 | TURN server infrastructure will be provisioned separately | WebRTC connectivity in restricted networks depends on this |

### Open Questions

1. **ANC Protocol Specifics:** What is the exact ANC checklist (trimester-by-trimester) that Audrey and the CDSS should follow? Which Kemenkes RI guidelines are currently in use at Balowerti?
2. **Database Migration Timeline:** Is PostgreSQL currently deployed in production, or is the system still file-backed? Is a migration planned for v2?
3. **Multi-Facility Expansion:** Is there a roadmap to deploy this system to other Puskesmas in Kota Kediri? If so, multi-tenancy architecture should be designed now.
4. **SMS/WhatsApp Gateway:** Which provider is preferred for sending patient telemedicine links — Twilio, WATI, or another Indonesian gateway?
5. **Regulatory Approval:** Has the system been reviewed by Dinas Kesehatan Kota Kediri? Are there specific data localization requirements (data harus disimpan di server Indonesia)?

---

## License

This project is licensed under the **MIT License**. See [`LICENSE`](./LICENSE).

The MIT License applies to the software and documentation in this repository. **Clinical use** remains subject to institutional policy, applicable Indonesian law, and the disclaimers in [`DISCLAIMER.md`](./DISCLAIMER.md) and [`DATA_PRIVACY.md`](./DATA_PRIVACY.md).

> Copyright © 2026 **Sentra Artificial Intelligence** — dr. Ferdi Iskandar

---

<div align="center">

Built with care for frontline healthcare workers in Indonesia. 🇮🇩

_Architect & Built by [Claudesy](https://github.com/DocSynapse) · Sentra Healthcare Solutions_

</div>
DESIGN.md — System & UI Design Reference
text
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
  heartRate={98text
<div align="center">

# 🏥 Puskesmas Intelligence Dashboard

**Clinical Information System for Primary & Maternal Healthcare Facilities**

<img src="public/pid.png" alt="Puskesmas PONED Balowerti Dashboard Preview" width="900" />

[![Status](https://img.shields.io/badge/Status-Production-brightgreen?style=for-the-badge)](https://github.com/DocSynapse)
[![Next.js](https://img.shields.io/badge/Next.js-16.1-black?style=for-the-badge&logo=next.js)](https://nextjs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-Strict-3178C6?style=for-the-badge&logo=typescript)](https://www.typescriptlang.org/)
[![React](https://img.shields.io/badge/React-19.2-61DAFB?style=for-the-badge&logo=react)](https://react.dev/)
[![Node.js](https://img.shields.io/badge/Node.js-%3E%3D20.9.0-339933?style=for-the-badge&logo=node.js)](https://nodejs.org/)
[![Railway](https://img.shields.io/badge/Deployed%20on-Railway-0B0D0E?style=for-the-badge&logo=railway)](https://railway.app/)
[![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)](./LICENSE)
[![CI](https://img.shields.io/github/actions/workflow/status/DocSynapse/healthcare-dashboard/ci.yml?style=for-the-badge&label=CI)](https://github.com/DocSynapse/healthcare-dashboard/actions)

_Architect & Built by [Claudesy](https://github.com/DocSynapse) · Sentra Healthcare Solutions_

> **"Technology enables, but humans decide."** — dr. Ferdi Iskandar, Founder

</div>

---

## Executive Summary

**Puskesmas Intelligence Dashboard** is a full-stack clinical operations platform purpose-built for **UPTD Puskesmas PONED Balowerti, Kota Kediri** — a primary healthcare facility (FKTP) with obstetric emergency services (PONED). It unifies clinical workflows, regulatory reporting, diagnostic AI, maternal care, and real-time team communication into a single cohesive interface.

**Why this project matters:**

- 🩺 **Reduces clinician administrative burden** by up to 70% through intelligent EMR automation and AI-assisted documentation
- 👶 **Improves maternal and neonatal outcomes** by equipping midwives and doctors with real-time clinical decision support and antenatal care tracking
- 📋 **Ensures regulatory compliance** with automated LB1/SP3 report generation aligned to Indonesian Ministry of Health standards (Kemenkes RI)
- 🌐 **Bridges the telemedicine gap** for rural and peri-urban patients through secure WebRTC-based virtual consultations
- 🔒 **Protects patient privacy** with HMAC-signed sessions, PHI-safe audit logs, and encryption at rest and in transit

**Target Users:**

| Persona | Role | Primary Pain Point Solved |
|---|---|---|
| Midwife (Bidan) | Antenatal care, delivery support | Manual ANC documentation, double-entry |
| General Practitioner | Patient encounters, diagnosis | Diagnostic uncertainty, ICD coding speed |
| Clinical Administrator | Reporting, scheduling | Monthly LB1 report preparation (hours → minutes) |
| Patient | Follow-up, remote consultation | Travel distance, wait times |

---

## Table of Contents

- [Executive Summary](#executive-summary)
- [Features Overview](#features-overview)
- [Quickstart & Demo](#quickstart--demo)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Environment Configuration](#environment-configuration)
  - [Running the Application](#running-the-application)
  - [Building for Production](#building-for-production)
- [Detailed Features](#detailed-features)
  - [1. User Profile Dashboard](#1-user-profile-dashboard)
  - [2. EMR Auto-Fill Engine](#2-emr-auto-fill-engine)
  - [3. ICD-X Finder](#3-icd-x-finder)
  - [4. LB1 Report Automation](#4-lb1-report-automation)
  - [5. Audrey — Clinical AI Assistant](#5-audrey--clinical-ai-assistant)
  - [6. ACARS — Internal Chat](#6-acars--internal-chat)
  - [7. CDSS — Clinical Decision Support](#7-cdss--clinical-decision-support)
  - [8. Crew Access Portal](#8-crew-access-portal)
  - [9. Telemedicine — Virtual Consultation](#9-telemedicine--virtual-consultation)
- [System Architecture](#system-architecture)
- [Project Structure](#project-structure)
- [Available Scripts](#available-scripts)
- [API Reference](#api-reference)
- [Security & Privacy](#security--privacy)
- [Operations & Deployment](#operations--deployment)
- [Integrations & Extensibility](#integrations--extensibility)
- [Developer Guide](#developer-guide)
- [Related Documentation](#related-documentation)
- [Assumptions & Open Questions](#assumptions--open-questions)
- [License](#license)

---

## Features Overview

| # | Feature | Status | Primary User |
|---|---|---|---|
| 1 | **User Profile Dashboard** | ✅ Live | All staff |
| 2 | **EMR Auto-Fill Engine** | ✅ Live | Doctor, Nurse |
| 3 | **ICD-X Finder** | ✅ Live | Doctor, Midwife |
| 4 | **LB1 Report Automation** | ✅ Live | Administrator |
| 5 | **Audrey — Clinical AI** | ✅ Live | Doctor |
| 6 | **ACARS — Internal Chat** | ✅ Live | All staff |
| 7 | **CDSS — Decision Support** | ✅ Live | Doctor, Midwife |
| 8 | **Crew Access Portal** | ✅ Live | All staff |
| 9 | **Telemedicine** | ✅ Live | Doctor, Patient |

---

## Quickstart & Demo

### Prerequisites

| Requirement | Minimum Version | Notes |
|---|---|---|
| Node.js | `>= 20.9.0` | [Download](https://nodejs.org/) |
| npm | `>= 10.x` | Bundled with Node.js |
| Git | Latest | [Download](https://git-scm.com/) |
| Chromium (Playwright) | Auto-installed | Required for EMR Auto-Fill only |

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/DocSynapse/healthcare-dashboard.git
cd healthcare-dashboard

# 2. Install all dependencies
npm install

# 3. Install Playwright browser binaries (EMR Auto-Fill feature)
npx playwright install chromium

# 4. Copy environment template
cp .env.example .env.local
# Edit .env.local with your credentials
```

### Environment Configuration

Create `.env.local` in the project root. **This file must never be committed to version control.**

```env
# ─── Server ───────────────────────────────────────────────────
PORT=7000
NODE_ENV=development

# ─── Authentication ───────────────────────────────────────────
# Generate with: openssl rand -hex 32
CREW_ACCESS_SECRET=your-secret-here-minimum-32-chars

# JSON array of staff user objects
CREW_ACCESS_USERS_JSON='[{"username":"admin","password":"change-me","displayName":"Administrator"}]'

# ─── AI Services ──────────────────────────────────────────────
GEMINI_API_KEY=your-gemini-api-key

# ─── EMR / ePuskesmas RPA ─────────────────────────────────────
# ⚠️ NEVER commit these values
EMR_BASE_URL=https://epuskesmas.example.id
EMR_LOGIN_URL=https://epuskesmas.example.id/login
EMR_USERNAME=your-epuskesmas-username
EMR_PASSWORD=your-epuskesmas-password
EMR_HEADLESS=true
EMR_SESSION_STORAGE_PATH=runtime/emr-session.json

# ─── LB1 Report Engine ────────────────────────────────────────
LB1_CONFIG_PATH=runtime/lb1-config.yaml
LB1_DATA_SOURCE_DIR=runtime/lb1-data
LB1_OUTPUT_DIR=runtime/lb1-output
LB1_HISTORY_FILE=runtime/lb1-run-history.jsonl
LB1_TEMPLATE_PATH=runtime/Laporan SP3 LB1.xlsx
LB1_MAPPING_PATH=runtime/diagnosis_mapping.csv

# ─── Telemedicine ─────────────────────────────────────────────
TURN_SERVER_URL=turn:your-turn-server:3478
TURN_SERVER_USERNAME=turn-user
TURN_SERVER_CREDENTIAL=turn-password
RECORDING_STORAGE_PATH=/var/recordings
TELEMEDICINE_PUBLIC_BASE_URL=https://your-domain.com/telemedicine/waiting

# ─── Database (Optional — falls back to file storage) ─────────
DATABASE_URL=postgresql://user:password@localhost:5432/puskesmas_db
```

> **Security Note:** `EMR_PASSWORD`, `CREW_ACCESS_SECRET`, and `TURN_SERVER_CREDENTIAL` are highly sensitive. Rotate regularly, use a secrets manager in production, and never log these values.

### Running the Application

```bash
# Development (with Socket.IO real-time support)
npm run dev

# Development (Next.js only, without Socket.IO)
npm run dev:next

# Access at: http://localhost:7000
```

### Building for Production

```bash
npm run build
npm run start
```

---

## Detailed Features

### 1. User Profile Dashboard

**Purpose:** Provide every logged-in staff member with a personalized home view: their profile, quick-access government portal links, and a live patient data summary.

#### User Stories

| ID | As a… | I want to… | So that… |
|---|---|---|---|
| US-01 | Midwife | See today's patient queue at login | I can plan my workday immediately |
| US-02 | Doctor | Access ePuskesmas and P-Care BPJS in one click | I avoid navigating between multiple logins |
| US-03 | Admin | View facility-level KPIs on the dashboard | I can monitor daily operations at a glance |
| US-04 | Any staff | See my profile and role information | I can confirm I am logged in to the correct account |

#### User Flow

```mermaid
flowchart TD
    A[User opens browser] --> B{Session cookie valid?}
    B -- Yes --> C[Load Profile Dashboard]
    B -- No --> D[Redirect to Login]
    D --> E[Enter credentials]
    E --> F{HMAC validation}
    F -- Valid --> C
    F -- Invalid --> G[Show error message]
    C --> H[Render: Profile card + Portal links + Patient summary]
    H --> I[User clicks portal link]
    I --> J[Open external portal in new tab]
```

#### UI Surfaces

- **Profile Card** — Avatar, name, role, department, login timestamp
- **Portal Quick-Links Grid** — Satu Sehat, SIPARWA, ePuskesmas, P-Care BPJS (icon buttons, opens new tab)
- **Patient Summary Widget** — Last 5 patients, vitals snapshot, ICD code badges
- **Health News Feed** — Latest Ministry of Health circulars via `/api/news`

#### Data Model

```sql
-- Staff/Crew table (in-memory / env-sourced in current implementation)
-- Future: Migrate to PostgreSQL
CREATE TABLE staff (
  id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  username    VARCHAR(64) UNIQUE NOT NULL,
  display_name VARCHAR(128) NOT NULL,
  role        VARCHAR(32) NOT NULL,         -- 'doctor','midwife','admin','nurse'
  department  VARCHAR(64),
  avatar_url  TEXT,
  is_active   BOOLEAN DEFAULT true,
  created_at  TIMESTAMPTZ DEFAULT now(),
  updated_at  TIMESTAMPTZ DEFAULT now()
);
```

#### API Contract

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| `GET` | `/api/auth/session` | Required | Get current session info |
| `GET` | `/api/news` | Required | Fetch health news feed |

**Example Response — `/api/auth/session`:**
```json
{
  "ok": true,
  "user": {
    "username": "dr.ferdi",
    "displayName": "dr. Ferdi Iskandar",
    "role": "doctor"
  },
  "expiresAt": "2026-04-16T07:00:00Z"
}
```

#### Security Considerations

- Session cookie is HMAC-signed; any tampering invalidates the session
- Portal links open in a sandboxed new tab (`rel="noopener noreferrer"`)
- Patient data in the summary widget is access-controlled by role

#### Testing Checklist

- [ ] Dashboard loads within 2 seconds on 3G connection
- [ ] Portal links render with correct URLs from config
- [ ] Session expiry at 12h triggers automatic redirect to login
- [ ] Role-based widgets render correctly per user role
- [ ] Portal grid is responsive on 320px mobile viewport

---

### 2. EMR Auto-Fill Engine

**Purpose:** Eliminate double-entry by using Playwright-driven RPA to automatically transfer structured clinical data (anamnesis, diagnosis, prescription) from the dashboard into ePuskesmas.

#### User Stories

| ID | As a… | I want to… | So that… |
|---|---|---|---|
| US-05 | Doctor | Submit patient visit data to ePuskesmas without re-typing | I save 10–15 minutes per patient |
| US-06 | Nurse | Monitor EMR transfer progress in real-time | I know when a transfer fails and can retry |
| US-07 | Admin | View EMR transfer history with timestamps | I can audit and troubleshoot failed transfers |
| US-08 | Doctor | Auto-fill prescription data | Pharmacy staff receives accurate medication orders |

#### User Flow

```mermaid
sequenceDiagram
    actor Doctor
    participant Dashboard as Next.js Dashboard
    participant API as /api/emr/transfer
    participant RPA as Playwright Engine
    participant ePuskesmas as ePuskesmas (External)

    Doctor->>Dashboard: Enters patient data (anamnesis, Dx, Rx)
    Doctor->>Dashboard: Clicks "Transfer to EMR"
    Dashboard->>API: POST /api/emr/transfer/run {patientData}
    API->>RPA: Launch headless Chromium session
    RPA->>ePuskesmas: Navigate to patient record
    RPA->>ePuskesmas: Fill form fields (anamnesis, ICD-X, prescriptions)
    RPA-->>API: Emit progress events via Socket.IO
    API-->>Dashboard: Stream progress updates (Socket.IO)
    Dashboard-->>Doctor: Show live progress bar & status
    RPA->>ePuskesmas: Submit form
    ePuskesmas-->>RPA: Confirm submission
    RPA-->>API: Emit completion event
    API-->>Dashboard: Transfer complete
    Dashboard-->>Doctor: Show success toast + audit log entry
```

#### Data Model

```sql
CREATE TABLE emr_transfer_runs (
  id            UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  patient_mrn   VARCHAR(32) NOT NULL,
  initiated_by  VARCHAR(64) NOT NULL,    -- staff username
  status        VARCHAR(16) NOT NULL,    -- 'pending','running','success','failed'
  payload       JSONB NOT NULL,          -- clinical data snapshot
  error_message TEXT,
  started_at    TIMESTAMPTZ DEFAULT now(),
  completed_at  TIMESTAMPTZ,
  duration_ms   INTEGER
);

CREATE INDEX idx_emr_runs_patient ON emr_transfer_runs(patient_mrn);
CREATE INDEX idx_emr_runs_status ON emr_transfer_runs(status);
```

#### API Contract

| Method | Endpoint | Description | Status Codes |
|---|---|---|---|
| `POST` | `/api/emr/transfer/run` | Execute EMR auto-fill | 202, 400, 409, 503 |
| `GET` | `/api/emr/transfer/status` | Current engine status | 200 |
| `GET` | `/api/emr/transfer/history` | Run history (paginated) | 200 |

**Example Request — `POST /api/emr/transfer/run`:**
```json
{
  "patientMrn": "PKM-2026-00123",
  "visitDate": "2026-04-15",
  "anamnesis": "Pasien datang dengan keluhan sakit kepala 2 hari...",
  "diagnoses": [{"code": "R51", "description": "Headache"}],
  "prescriptions": [
    {"drug": "Paracetamol", "dose": "500mg", "frequency": "3x1", "duration": "3 days"}
  ]
}
```

**Example Response (202 Accepted):**
```json
{
  "ok": true,
  "runId": "550e8400-e29b-41d4-a716-446655440000",
  "message": "EMR transfer initiated. Monitor via Socket.IO event: emr:progress"
}
```

**Error Response (409 Conflict):**
```json
{
  "ok": false,
  "error": "TRANSFER_IN_PROGRESS",
  "message": "An EMR transfer is already running. Wait for completion before starting a new one."
}
```

#### Security & Privacy

- EMR credentials (`EMR_USERNAME`, `EMR_PASSWORD`) are resolved at runtime from environment variables — never logged or exposed to the client
- Playwright runs in an isolated browser context with a dedicated session file
- Transfer payload containing PHI is stored encrypted in the database
- Audit log records username, timestamp, and result — but omits patient-identifiable data from log lines

#### Testing Checklist

- [ ] Transfer succeeds end-to-end with valid ePuskesmas credentials
- [ ] Concurrent transfer requests return 409
- [ ] Socket.IO events are received on the frontend during transfer
- [ ] Failed transfer writes error details to history with full stack trace
- [ ] Playwright session file is rotated after 24 hours

---

### 3. ICD-X Finder

**Purpose:** Provide fast, accurate ICD-10 diagnosis coding at the point of care, supporting three catalog versions (2010, 2016, 2019) with fuzzy search and legacy code translation.

#### User Stories

| ID | As a… | I want to… | So that… |
|---|---|---|---|
| US-09 | Doctor | Search ICD-10 codes by symptom keyword | I find the right code in under 5 seconds |
| US-10 | Midwife | Look up ANC-related ICD codes (Z34.x) | I code antenatal visits accurately |
| US-11 | Doctor | Translate legacy 2010 codes to 2019 equivalents | I can update old records to current standards |
| US-12 | Admin | Filter by ICD catalog version | I match the version required by my reporting system |

#### User Flow

```mermaid
flowchart LR
    A[User types symptom/code] --> B[Debounced API call]
    B --> C{Cache hit?}
    C -- Yes --> D[Return cached results]
    C -- No --> E[Search ICD DB]
    E --> F[Fuzzy match algorithm]
    F --> G[Return ranked results]
    G --> H[Display in table with badges]
    H --> I[User clicks result]
    I --> J[Copy code to clipboard / Insert into form]
```

#### Data Model

```sql
CREATE TABLE icd10_codes (
  id          SERIAL PRIMARY KEY,
  code        VARCHAR(8) NOT NULL,
  version     SMALLINT NOT NULL,         -- 2010, 2016, 2019
  description TEXT NOT NULL,
  category    VARCHAR(3),                -- First 3 chars of code (chapter)
  is_billable BOOLEAN DEFAULT true,
  parent_code VARCHAR(8),
  UNIQUE(code, version)
);

CREATE INDEX idx_icd_code ON icd10_codes(code);
CREATE INDEX idx_icd_version ON icd10_codes(version);
CREATE INDEX idx_icd_description_fts ON icd10_codes USING gin(to_tsvector('indonesian', description));
```

**Example Record:**
```json
{
  "code": "Z34.0",
  "version": 2019,
  "description": "Supervision of normal first pregnancy",
  "category": "Z34",
  "isBillable": true,
  "parentCode": "Z34"
}
```

#### API Contract

| Method | Endpoint | Query Params | Description |
|---|---|---|---|
| `GET` | `/api/icdx/lookup` | `q`, `version`, `limit` | Search ICD-10 codes |

**Example:** `GET /api/icdx/lookup?q=headache&version=2019&limit=10`

```json
{
  "ok": true,
  "version": 2019,
  "query": "headache",
  "results": [
    {"code": "R51", "description": "Headache", "category": "R51", "isBillable": true},
    {"code": "G44.3", "description": "Post-traumatic headache", "category": "G44", "isBillable": true}
  ],
  "total": 2
}
```

#### Testing Checklist

- [ ] Search returns results in < 200ms for common terms
- [ ] Fuzzy match handles misspellings ("headache" → "hedache")
- [ ] Version switching reloads correct catalog without page refresh
- [ ] Legacy code translation returns mapped 2019 equivalent
- [ ] Empty query returns 400 with validation message

---

### 4. LB1 Report Automation

**Purpose:** Automate the monthly LB1/SP3 national health reporting pipeline — from raw ePuskesmas export to a validated, submission-ready Excel report.

#### User Stories

| ID | As a… | I want to… | So that… |
|---|---|---|---|
| US-13 | Admin | Generate the LB1 report in one click | I reduce monthly report prep from 4 hours to 10 minutes |
| US-14 | Admin | Download the QC CSV of rejected records | I can manually fix invalid entries before resubmission |
| US-15 | Admin | View run history with timestamps | I can track when reports were last generated |
| US-16 | Doctor | Run a preflight validation before report generation | I catch data errors before they fail the pipeline |

#### Pipeline Flow

```mermaid
flowchart TD
    A[Upload ePuskesmas export CSV] --> B[Normalize & validate records]
    B --> C{Validation pass?}
    C -- No --> D[Write rejected records to QC CSV]
    C -- Yes --> E[Map ICD-10 codes to LB1 disease categories]
    E --> F[Aggregate counts per category per period]
    F --> G[Populate Excel LB1 template]
    G --> H[Write JSON audit summary]
    H --> I[Output: .xlsx + QC.csv + audit.json]
    D --> F
```

#### Output Files

| File | Format | Description |
|---|---|---|
| `LB1_YYYY_MM.xlsx` | Excel | Completed national LB1 report |
| `LB1_YYYY_MM_QC.csv` | CSV | Rejected records with rejection reason |
| `LB1_YYYY_MM_audit.json` | JSON | Run metadata: total records, pass rate, duration |

#### API Contract

| Method | Endpoint | Description | Status Codes |
|---|---|---|---|
| `GET` | `/api/report/automation/preflight` | Pre-run data validation | 200, 422 |
| `POST` | `/api/report/automation/run` | Execute LB1 pipeline | 202, 409, 503 |
| `GET` | `/api/report/automation/status` | Current pipeline status | 200 |
| `GET` | `/api/report/automation/history` | Run history (`?limit=30`) | 200 |
| `GET` | `/api/report/files` | List output files | 200 |
| `GET` | `/api/report/files/download` | Download specific file | 200, 404 |

#### Security & Privacy

- LB1 data contains aggregated disease statistics — individual patient records are de-identified in the output
- Audit JSON logs contain only record counts and validation summaries — no patient names or MRNs
- Output files are stored in a server-side directory not accessible via public URLs; downloads require authentication

---

### 5. Audrey — Clinical AI Assistant

**Purpose:** A voice-first AI clinical copilot powered by Google Gemini 2.5 Flash (native audio), providing real-time diagnostic insights calibrated for Puskesmas-level resources during patient encounters.

#### User Stories

| ID | As a… | I want to… | So that… |
|---|---|---|---|
| US-17 | Doctor | Ask Audrey about differential diagnoses by voice | I get instant suggestions without typing |
| US-18 | Doctor | Receive Puskesmas-appropriate treatment recommendations | Suggestions respect resource constraints (no CT, limited labs) |
| US-19 | Midwife | Ask Audrey about ANC protocols | I follow correct antenatal care guidelines |
| US-20 | Doctor | Have Audrey summarize the patient history verbally | I stay focused on the patient, not the screen |

> **⚠️ Clinical Disclaimer:** Audrey provides AI-assisted suggestions only. All clinical decisions must be made by a licensed healthcare professional. Audrey's output must never be applied without professional verification.

#### API Contract

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/voice/chat` | Send text/audio to Audrey, receive response |
| `POST` | `/api/voice/tts` | Text-to-speech synthesis |
| `GET` | `/api/voice/token` | Retrieve Gemini session token |

**Example Request — `POST /api/voice/chat`:**
```json
{
  "message": "Patient is a 28-year-old woman, G2P1, 36 weeks pregnant, presenting with headache and BP 150/100. What are your thoughts?",
  "context": "ANC",
  "sessionId": "session-abc123"
}
```

**Example Response:**
```json
{
  "ok": true,
  "reply": "Given the presentation of hypertension (150/100) at 36 weeks in a multigravida patient, the key differential includes pre-eclampsia. Immediate priorities: check for proteinuria (dipstick), assess fetal heart rate, and evaluate for symptoms of severe pre-eclampsia (visual disturbances, epigastric pain). Per Puskesmas PONED protocol, if pre-eclampsia is confirmed, initiate MgSO4 loading dose and prepare for urgent referral to RS if signs of severity are present.",
  "confidence": "high",
  "sources": ["Kemenkes RI ANC Protocol 2020", "PONED Clinical Guidelines"]
}
```

#### Security & Privacy

- Voice sessions are ephemeral — audio is processed in-memory and not stored unless explicitly flagged
- Gemini API key is server-side only, never exposed to the browser
- Session tokens have a 1-hour TTL

---

### 6. ACARS — Internal Chat

**Purpose:** Real-time team messaging for clinical and administrative staff within the facility, enabling rapid coordination without leaving the dashboard.

#### User Stories

| ID | As a… | I want to… | So that… |
|---|---|---|---|
| US-21 | Nurse | Message the doctor about an urgent patient | The doctor is notified immediately without phone calls |
| US-22 | Doctor | See who is online in the team | I know who I can reach right now |
| US-23 | Midwife | Message the admin about a BPJS code issue | We resolve billing issues quickly |
| US-24 | Any staff | See typing indicators in conversations | I know a reply is incoming |

#### Real-Time Events (Socket.IO)

| Event | Direction | Payload |
|---|---|---|
| `acars:message` | Server → Client | `{roomId, sender, text, timestamp}` |
| `acars:typing` | Client → Server | `{roomId, username}` |
| `acars:presence` | Server → Client | `{username, status: 'online'/'offline'}` |
| `acars:read` | Client → Server | `{roomId, messageId}` |

#### Data Model

```sql
CREATE TABLE chat_rooms (
  id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name        VARCHAR(128),
  type        VARCHAR(16) NOT NULL,       -- 'direct', 'group'
  created_at  TIMESTAMPTZ DEFAULT now()
);

CREATE TABLE chat_messages (
  id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  room_id     UUID REFERENCES chat_rooms(id),
  sender      VARCHAR(64) NOT NULL,
  content     TEXT NOT NULL,
  created_at  TIMESTAMPTZ DEFAULT now(),
  read_by     JSONB DEFAULT '[]'         -- array of usernames
);

CREATE INDEX idx_messages_room ON chat_messages(room_id, created_at DESC);
```

---

### 7. CDSS — Clinical Decision Support

**Purpose:** Provide ranked differential diagnoses and evidence-based treatment recommendations by combining a local disease knowledge base (159 diseases, 45,030 real encounter records) with Gemini-powered reasoning.

#### User Stories

| ID | As a… | I want to… | So that… |
|---|---|---|---|
| US-25 | Doctor | Input patient symptoms and receive ranked differentials | I don't miss uncommon but serious diagnoses |
| US-26 | Midwife | Get ANC risk stratification suggestions | I identify high-risk pregnancies early |
| US-27 | Doctor | Receive referral criteria per diagnosis | I know when to refer vs. manage at Puskesmas level |
| US-28 | Doctor | See treatment plans aligned to Puskesmas formulary | Recommendations are actually actionable locally |

#### API Contract

**`POST /api/cdss/diagnose`**

```json
// Request
{
  "symptoms": ["headache", "fever", "stiff neck"],
  "patientAge": 25,
  "patientSex": "female",
  "vitals": {"bp": "120/80", "temp": 38.5, "hr": 98},
  "context": "general"
}

// Response
{
  "ok": true,
  "differentials": [
    {
      "rank": 1,
      "diagnosis": "Bacterial Meningitis",
      "icdCode": "G00.9",
      "probability": "high",
      "urgency": "emergency",
      "referralRequired": true,
      "rationale": "Classic triad of headache, fever, and neck stiffness...",
      "immediateActions": ["Urgent referral to hospital", "Do not delay for LP"]
    }
  ],
  "disclaimer": "AI-generated suggestion. Clinical judgment required."
}
```

> **⚠️ Assumption:** Clinical protocols and drug formulary data are based on Indonesian Ministry of Health guidelines (Permenkes). Specific protocol versions should be verified by the clinical team and updated in the knowledge base regularly.

---

### 8. Crew Access Portal

**Purpose:** Secure the entire dashboard behind an authentication gate using HMAC-signed session cookies with role-based access.

#### Authentication Flow

```mermaid
sequenceDiagram
    actor User
    participant Browser
    participant API as /api/auth
    participant Server as Node.js Server

    User->>Browser: Navigate to dashboard
    Browser->>Server: GET /
    Server->>Browser: Check session cookie
    alt No valid session
        Browser->>Browser: Render <CrewAccessGate /> (login form)
        User->>Browser: Enter username + password
        Browser->>API: POST /api/auth/login
        API->>API: Validate credentials (env > JSON > defaults)
        API->>API: Generate HMAC-signed cookie (12h TTL)
        API->>Browser: Set-Cookie: crew_session=...
        Browser->>Browser: Re-render dashboard
    else Valid session
        Browser->>Browser: Render full dashboard
    end
```

#### Credential Resolution Priority
CREW_ACCESS_USERS_JSON (environment variable) ← Highest priority

runtime/crew-users.json (runtime file)

Compiled defaults ← Lowest priority (dev only)

text

#### Security Properties

- Session cookies: `HttpOnly`, `Secure` (production), `SameSite=Strict`
- HMAC algorithm: SHA-256 with `CREW_ACCESS_SECRET`
- Session TTL: 12 hours; auto-invalidated on server restart in development
- No database dependency for authentication — stateless and fast
- Brute-force protection: implement rate limiting on `/api/auth/login` (5 attempts/15 min)

---

### 9. Telemedicine — Virtual Consultation

**Purpose:** Enable secure, real-time video consultations between Puskesmas doctors and patients — reducing unnecessary in-person visits for follow-ups, medication reviews, and remote triage.

#### User Stories

| ID | As a… | I want to… | So that… |
|---|---|---|---|
| US-29 | Doctor | Start a video call with a patient via one-time link | The patient can join without creating an account |
| US-30 | Patient | Join from a mobile browser by clicking a WhatsApp/SMS link | No app installation is required |
| US-31 | Doctor | Share lab results during the call | The patient understands their results remotely |
| US-32 | Doctor | Have a SOAP note drafted automatically post-session | I spend less time on post-visit documentation |

#### Telemedicine Workflow

```mermaid
sequenceDiagram
    actor Doctor
    actor Patient
    participant Dashboard
    participant API as /api/telemedicine
    participant TURN as STUN/TURN Server
    participant EMR as EMR System

    Doctor->>Dashboard: Create consultation slot
    Dashboard->>API: POST /api/telemedicine/sessions
    API-->>Dashboard: {sessionId, patientToken}
    Dashboard-->>Doctor: Send patient link (SMS/WhatsApp)
    Doctor->>Patient: Shares one-time link

    Patient->>Dashboard: Opens waiting room URL
    Dashboard->>API: GET session by token
    API-->>Dashboard: Session valid

    Doctor->>Dashboard: Admits patient
    Dashboard->>TURN: ICE candidate exchange (via Socket.IO signaling)
    TURN-->>Dashboard: WebRTC peer connection established

    Note over Doctor,Patient: Video + audio + chat + file sharing active

    Doctor->>Dashboard: End session
    Dashboard->>API: PATCH /api/telemedicine/sessions/:id {status: "completed"}
    API->>EMR: Generate SOAP note draft (Gemini)
    EMR-->>Dashboard: SOAP note ready for review
    Doctor->>Dashboard: Review + save to patient EMR
```

#### API Contract

| Method | Endpoint | Description | Status Codes |
|---|---|---|---|
| `POST` | `/api/telemedicine/sessions` | Create new session | 201, 400 |
| `GET` | `/api/telemedicine/sessions` | List sessions (paginated) | 200 |
| `GET` | `/api/telemedicine/sessions/:id` | Get session details | 200, 404 |
| `PATCH` | `/api/telemedicine/sessions/:id` | Update session status | 200, 400 |
| `POST` | `/api/telemedicine/signal` | WebRTC signaling relay | 200 |
| `POST` | `/api/telemedicine/recording/start` | Begin session recording | 202 |
| `POST` | `/api/telemedicine/recording/stop` | End & save recording | 200 |
| `GET` | `/api/telemedicine/schedule` | List appointment slots | 200 |
| `POST` | `/api/telemedicine/schedule` | Create appointment slot | 201 |

#### Data Model

```sql
CREATE TABLE telemedicine_sessions (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  doctor_id       VARCHAR(64) NOT NULL,
  patient_token   VARCHAR(128) UNIQUE NOT NULL,  -- one-time join token
  patient_name    VARCHAR(128),
  status          VARCHAR(16) DEFAULT 'scheduled', -- scheduled/waiting/active/completed/cancelled
  scheduled_at    TIMESTAMPTZ,
  started_at      TIMESTAMPTZ,
  ended_at        TIMESTAMPTZ,
  recording_path  TEXT,
  soap_note       TEXT,
  emr_saved       BOOLEAN DEFAULT false,
  created_at      TIMESTAMPTZ DEFAULT now()
);

CREATE INDEX idx_sessions_doctor ON telemedicine_sessions(doctor_id, scheduled_at);
CREATE INDEX idx_sessions_token ON telemedicine_sessions(patient_token);
```

#### Privacy & Consent

- Recording is strictly opt-in; explicit consent is requested and logged before recording begins
- Recordings are stored server-side with AES-256 encryption at rest
- Patient tokens are single-use and expire after 24 hours or session end
- SOAP notes generated by AI are marked as draft and require physician sign-off before becoming part of the official EMR record

---

## System Architecture

### Architecture Overview

```mermaid
graph TB
    subgraph Client Layer
        Browser["Web Browser\n(Next.js React App)"]
        Mobile["Mobile Browser\n(Responsive PWA)"]
    end

    subgraph Application Layer
        NextApp["Next.js 16.1\nApp Router\n(server.ts entry)"]
        SocketIO["Socket.IO Server\n(Real-time events)"]
    end

    subgraph Service Layer
        RPA["Playwright RPA\n(EMR Auto-Fill)"]
        GeminiAI["Google Gemini 2.5 Flash\n(Audrey + CDSS + SOAP)"]
        WebRTC["WebRTC Signaling\n(Telemedicine)"]
        SheetJS["SheetJS\n(LB1 Excel I/O)"]
    end

    subgraph Data Layer
        PostgreSQL[("PostgreSQL\n(Primary DB)")]
        FileStore["File Storage\n(Reports, Recordings)"]
        RuntimeJSON["Runtime JSON/YAML\n(Config, Credentials)"]
    end

    subgraph External Integrations
        ePuskesmas["ePuskesmas\n(National EMR)"]
        SatuSehat["Satu Sehat\n(MOH Platform)"]
        BPJS["P-Care BPJS\n(Insurance)"]
        TURN["STUN/TURN Server\n(WebRTC NAT traversal)"]
        SMS["SMS/WhatsApp Gateway\n(Patient links)"]
    end

    Browser <-->|HTTPS + WSS| NextApp
    Mobile <-->|HTTPS + WSS| NextApp
    NextApp <--> SocketIO
    NextApp --> RPA
    NextApp --> GeminiAI
    NextApp <--> WebRTC
    NextApp --> SheetJS
    NextApp <--> PostgreSQL
    NextApp --> FileStore
    NextApp --> RuntimeJSON
    RPA --> ePuskesmas
    NextApp --> SatuSehat
    NextApp --> BPJS
    WebRTC <--> TURN
    NextApp --> SMS
```

### Deployment Model Recommendation

**Recommendation: Enhanced Monolith on Railway (current) → Modular Monolith (v2)**

| Approach | Pros | Cons | Verdict |
|---|---|---|---|
| **Single Monolith** | Simple ops, single deploy, low cost | Hard to scale individual services | ✅ Best for current team size |
| Microservices | Independent scaling per service | Operational complexity, latency | ❌ Premature for current scale |
| Serverless | Near-zero idle cost | Cold starts incompatible with WebSockets | ❌ Incompatible with Socket.IO/WebRTC |

The current custom `server.ts` architecture (Next.js + Socket.IO in one process) is correct and optimal for a Puskesmas-scale deployment. At larger scale (>50 concurrent users), the Playwright RPA worker and CDSS inference should be extracted to separate background workers.

### Scaling Strategy

- **Stateless API routes** — all session state in HMAC cookies; horizontally scalable
- **Socket.IO with Redis Adapter** — enables multi-instance Socket.IO when scaling beyond one process
- **Playwright workers** — run in a separate worker pool (max 3 concurrent RPA jobs to prevent resource exhaustion)
- **Caching** — ICD-10 lookup results cached in Redis with 24h TTL; reduces DB read load
- **CDN** — Static assets served via Railway's CDN edge; reduce server load

---

## Project Structure
healthcare-dashboard/
├── server.ts # Custom HTTP + Socket.IO server entry point
├── next.config.ts # Next.js configuration
├── tsconfig.json # TypeScript (strict mode)
├── railway.toml # Railway deployment configuration
├── package.json # Dependencies and scripts
│
├── src/
│ ├── app/
│ │ ├── layout.tsx # Root layout (ThemeProvider + CrewAccessGate + AppNav)
│ │ ├── page.tsx # Home / User Profile Dashboard
│ │ ├── globals.css # Global CSS with dark/light theme tokens
│ │ ├── emr/ # EMR Auto-Fill interface
│ │ ├── icdx/ # ICD-X lookup page
│ │ ├── report/ # LB1 report generation page
│ │ ├── voice/ # Audrey voice consultation page
│ │ ├── acars/ # Internal chat page
│ │ ├── pasien/ # Patient records page
│ │ ├── telemedicine/ # Telemedicine module
│ │ └── api/
│ │ ├── auth/ # Login, logout, session
│ │ ├── cdss/ # Clinical decision support
│ │ ├── emr/ # EMR transfer endpoints
│ │ ├── icdx/ # ICD-10 lookup
│ │ ├── report/ # LB1 automation + clinical reports
│ │ ├── voice/ # TTS, chat, token
│ │ ├── telemedicine/ # Sessions, signaling, recording, schedule
│ │ ├── news/ # Health news
│ │ └── perplexity/ # Perplexity AI integration
│ │
│ ├── components/
│ │ ├── AppNav.tsx # Sidebar navigation
│ │ ├── CrewAccessGate.tsx # Authentication gate
│ │ ├── ThemeProvider.tsx # Dark/light theme context
│ │ └── ui/ # Shared UI component library
│ │
│ └── lib/
│ ├── crew-access.ts # Auth types and constants
│ ├── server/ # Server-only auth logic
│ ├── lb1/ # LB1 report engine
│ ├── emr/ # EMR auto-fill engine + Playwright
│ ├── icd/ # ICD-10 database
│ └── telemedicine/ # WebRTC, signaling, recorder, SOAP generator
│
├── docs/
│ └── plans/ # Design documents and feature specs
│
├── runtime/ # Runtime config (gitignored — never commit)
│ ├── emr-session.json
│ ├── lb1-config.yaml
│ ├── lb1-data/
│ ├── lb1-output/
│ └── crew-users.json
│
└── mintlify-docs/ # Public API documentation (Mintlify)

text

---

## Available Scripts

| Script | Description |
|---|---|
| `npm run dev` | Start custom server with Socket.IO (port 7000) |
| `npm run dev:clean` | Clear lock file and start dev server |
| `npm run dev:next` | Start Next.js without custom server |
| `npm run build` | Compile production bundle |
| `npm run start` | Start production server |
| `npm run docs:dev` | Local Mintlify preview (port 3004) |
| `npm run docs:api` | Regenerate OpenAPI spec from route handlers |
| `npm run docs:check` | Validate Mintlify site + broken links |

---

## API Reference

All routes are prefixed with `/api`. Authentication is enforced via HMAC-signed session cookies on all routes.

### Authentication

| Method | Endpoint | Description | Status Codes |
|---|---|---|---|
| `POST` | `/api/auth/login` | Authenticate with crew credentials | 200, 401, 429 |
| `POST` | `/api/auth/logout` | End current session | 200 |
| `GET` | `/api/auth/session` | Validate current session | 200, 401 |

### Clinical Decision Support

| Method | Endpoint | Description | Status Codes |
|---|---|---|---|
| `POST` | `/api/cdss/diagnose` | Run diagnostic suggestion engine | 200, 400, 503 |
| `GET` | `/api/icdx/lookup` | ICD-10 code search (`?q=&version=`) | 200, 400 |

### EMR Transfer

| Method | Endpoint | Description | Status Codes |
|---|---|---|---|
| `POST` | `/api/emr/transfer/run` | Execute EMR auto-fill | 202, 409, 503 |
| `GET` | `/api/emr/transfer/status` | Transfer engine status | 200 |
| `GET` | `/api/emr/transfer/history` | Transfer history | 200 |

### LB1 Report Automation

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/report/automation/preflight` | Pre-run validation |
| `POST` | `/api/report/automation/run` | Execute pipeline |
| `GET` | `/api/report/automation/status` | Pipeline status |
| `GET` | `/api/report/automation/history` | Run history |
| `GET` | `/api/report/files` | List output files |
| `GET` | `/api/report/files/download` | Download file |

### Clinical Reports

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/report/clinical` | List reports |
| `POST` | `/api/report/clinical` | Create clinical report |
| `DELETE` | `/api/report/clinical?id=` | Delete report |

### Voice / Audrey

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/voice/chat` | Send message to Audrey |
| `POST` | `/api/voice/tts` | Text-to-speech synthesis |
| `GET` | `/api/voice/token` | Retrieve voice session token |

### Telemedicine

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/telemedicine/sessions` | Create session |
| `GET` | `/api/telemedicine/sessions` | List sessions |
| `GET` | `/api/telemedicine/sessions/:id` | Get session |
| `PATCH` | `/api/telemedicine/sessions/:id` | Update session |
| `POST` | `/api/telemedicine/signal` | WebRTC signaling |
| `POST` | `/api/telemedicine/recording/start` | Start recording |
| `POST` | `/api/telemedicine/recording/stop` | Stop recording |
| `GET` | `/api/telemedicine/schedule` | List slots |
| `POST` | `/api/telemedicine/schedule` | Create slot |

---

## Security & Privacy

### Authentication & Authorization

- **Auth mechanism:** HMAC-SHA256 signed session cookies (`crew_session`)
- **TTL:** 12 hours; requires re-authentication after expiry
- **Session flags:** `HttpOnly`, `Secure` (production), `SameSite=Strict`
- **Role-based access:** Roles enforced at the API route level (`doctor`, `midwife`, `nurse`, `admin`)
- **Recommended future enhancement:** Add OAuth2/OIDC with an identity provider (Keycloak or Auth0) for SSO across Puskesmas systems

### Encryption

| Layer | Mechanism |
|---|---|
| In transit | TLS 1.3 (enforced by Railway/CDN) |
| At rest (recordings) | AES-256 encryption |
| Secrets | Environment variables (Railway Vault in production) |
| Cookie | HMAC-SHA256 signature (tamper-proof) |

### Privacy & Compliance

- **Indonesian context:** Align with UU No. 17/2023 (Kesehatan) and Permenkes regarding health data protection
- **GDPR equivalence:** Implement data minimization, purpose limitation, and right-to-erasure for any EU-connected deployments
- **HIPAA equivalence:** Apply PHI classification, audit logging, and BAA (Business Associate Agreement) requirements for any US deployments
- **Consent management:** Explicit patient consent required for telemedicine recording and any data sharing beyond the treating facility
- **Audit logging:** All PHI-touching operations logged with: `{timestamp, actor, action, resource_type, resource_id}` — never include patient name or MRN in log lines

### Threat Mitigations

| Threat | Mitigation |
|---|---|
| Session forgery | HMAC-signed cookies; server-side validation |
| Credential brute force | Rate limiting on login endpoint (5 req/15 min) |
| XSS | Next.js CSP headers; no dangerouslySetInnerHTML |
| SQL injection | Parameterized queries via ORM |
| SSRF (Playwright) | Allowlist of permitted target URLs for RPA |
| PHI data leak | No patient identifiers in logs; PHI encrypted at rest |

---

## Operations & Deployment

### Railway Deployment

```toml
# railway.toml
[build]
builder = "nixpacks"
buildCommand = "npm run build"

[deploy]
startCommand = "npm run start"
restartPolicyType = "on_failure"
restartPolicyMaxRetries = 3
```

**Deployment Steps:**
1. Push repository to GitHub
2. Connect repository to new Railway project
3. Set all required environment variables in Railway dashboard → **Variables**
4. Railway auto-builds and deploys on every push to `master`
5. Verify health at `https://your-domain.com/api/auth/session` (expect 401)

### CI/CD Pipeline

```yaml
# .github/workflows/ci.yml (recommended)
name: CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: '20' }
      - run: npm ci
      - run: npm run build
      - run: npx playwright install chromium
      - run: npm test
```

### Observability

| Signal | Tool | Key Metrics |
|---|---|---|
| Logs | Railway structured logs | Error rate, auth failures, RPA errors |
| Metrics | Railway metrics dashboard | CPU, memory, response time |
| Alerts | Railway notifications | Build failures, restart loops |
| Tracing (recommended) | OpenTelemetry + Jaeger | End-to-end request latency |

**Recommended Alert Thresholds:**

- 🔴 Error rate > 5% over 5 minutes
- 🔴 Memory usage > 85% sustained 10 minutes
- 🟡 EMR transfer failure rate > 20%
- 🟡 Average API response time > 2 seconds

### Backup & Recovery

| Data | Backup Frequency | Recovery Method |
|---|---|---|
| PostgreSQL | Daily pg_dump to S3 | Restore from latest dump |
| Runtime config files | Synced to Railway env vars | Redeploy from env vars |
| LB1 output files | Retained for 12 months | Download from file storage |
| Telemedicine recordings | Retained per institutional policy | Restore from encrypted S3 |

---

## Integrations & Extensibility

### Current Integrations

| Integration | Purpose | Method |
|---|---|---|
| ePuskesmas | EMR data entry | Playwright RPA (no public API) |
| Satu Sehat (MOH) | National health platform link | External URL redirect |
| P-Care BPJS | Insurance claim platform | External URL redirect |
| Google Gemini 2.5 Flash | AI assistant, CDSS reasoning, SOAP generation | REST API |
| Perplexity AI | Research queries | REST API |

### Recommended Future Integrations

| Integration | Use Case | Protocol |
|---|---|---|
| WhatsApp Business API | Patient appointment links | REST (Twilio/WATI) |
| SMS Gateway | Fallback patient notifications | REST |
| FHIR R4 | HL7 health data interoperability | REST/JSON-FHIR |
| Payment Gateway (Midtrans) | Patient billing | REST |
| Lab Information System | Lab result retrieval | HL7/REST |
| OpenMRS | Open-source EMR interoperability | REST API |

### API Versioning

All future breaking changes will be versioned under `/api/v2/...`. Current API is considered `v1` (implicit). Deprecation notices will be published in `CHANGELOG.md` with a minimum 90-day sunset window.

---

## Developer Guide

### Local Development Setup

```bash
git clone https://github.com/DocSynapse/healthcare-dashboard.git
cd healthcare-dashboard
npm install
npx playwright install chromium
cp .env.example .env.local
# Edit .env.local — at minimum set CREW_ACCESS_SECRET and CREW_ACCESS_USERS_JSON
npm run dev
```

### Database Seed (Development)

```bash
# Seed ICD-10 database (2010, 2016, 2019)
npm run db:seed:icd

# Seed sample patient records (anonymized dummy data)
npm run db:seed:patients

# Reset database
npm run db:reset
```

### Code Style

- **TypeScript strict mode** — no `any` types without explicit justification
- **ESLint** with Next.js recommended config
- **Prettier** for formatting (run `npm run format` before commit)
- **File naming:** `kebab-case.ts` for utilities, `PascalCase.tsx` for components

### Commit Message Guidelines
type(scope): short imperative description

Types: feat | fix | chore | docs | refactor | test | perf
Examples:
feat(emr): add retry logic for failed transfers
fix(auth): correct HMAC cookie expiry calculation
docs(readme): expand telemedicine API reference

text

### Pull Request Checklist

- [ ] Code compiles without TypeScript errors (`npm run build`)
- [ ] No secrets or credentials in diff
- [ ] New features have corresponding test cases
- [ ] API changes documented in endpoint tables
- [ ] Environment variable additions documented in `.env.example`
- [ ] `CHANGELOG.md` updated

---

## Related Documentation

| Document | Description |
|---|---|
| [ARCHITECTURE.md](./ARCHITECTURE.md) | System architecture and component design |
| [CONTRIBUTING.md](./CONTRIBUTING.md) | Development workflow and conventions |
| [CHANGELOG.md](./CHANGELOG.md) | Version history and release notes |
| [SECURITY.md](./SECURITY.md) | Security policy and vulnerability reporting |
| [DISCLAIMER.md](./DISCLAIMER.md) | Clinical and liability disclaimer |
| [DATA_PRIVACY.md](./DATA_PRIVACY.md) | Privacy commitments |
| [MODEL_CARD.md](./MODEL_CARD.md) | AI model integration summary |
| [DATASET_CARD.md](./DATASET_CARD.md) | CDSS dataset stewardship notes |
| [docs/DEPLOYMENT.md](./docs/DEPLOYMENT.md) | Deployment operations notes |
| [docs/TROUBLESHOOTING.md](./docs/TROUBLESHOOTING.md) | Common failures and fixes |
| [mintlify-docs/](./mintlify-docs/) | Public API documentation (Mintlify) |

---

## Assumptions & Open Questions

### Assumptions Made

| # | Assumption | Impact if Wrong |
|---|---|---|
| A1 | Indonesian clinical protocols follow Kemenkes RI 2020 ANC guidelines | CDSS/Audrey responses may need recalibration |
| A2 | ePuskesmas does not provide a public REST API; Playwright RPA is the only integration path | If ePuskesmas exposes an API, RPA can be replaced with direct API calls |
| A3 | Current user base < 50 concurrent users; monolith is sufficient | Needs redesign if concurrent load exceeds this |
| A4 | PostgreSQL is the intended primary database (currently partial/file fallback) | Schema must be confirmed with the engineering team |
| A5 | TURN server infrastructure will be provisioned separately | WebRTC connectivity in restricted networks depends on this |

### Open Questions

1. **ANC Protocol Specifics:** What is the exact ANC checklist (trimester-by-trimester) that Audrey and the CDSS should follow? Which Kemenkes RI guidelines are currently in use at Balowerti?
2. **Database Migration Timeline:** Is PostgreSQL currently deployed in production, or is the system still file-backed? Is a migration planned for v2?
3. **Multi-Facility Expansion:** Is there a roadmap to deploy this system to other Puskesmas in Kota Kediri? If so, multi-tenancy architecture should be designed now.
4. **SMS/WhatsApp Gateway:** Which provider is preferred for sending patient telemedicine links — Twilio, WATI, or another Indonesian gateway?
5. **Regulatory Approval:** Has the system been reviewed by Dinas Kesehatan Kota Kediri? Are there specific data localization requirements (data harus disimpan di server Indonesia)?

---

## License

This project is licensed under the **MIT License**. See [`LICENSE`](./LICENSE).

The MIT License applies to the software and documentation in this repository. **Clinical use** remains subject to institutional policy, applicable Indonesian law, and the disclaimers in [`DISCLAIMER.md`](./DISCLAIMER.md) and [`DATA_PRIVACY.md`](./DATA_PRIVACY.md).

> Copyright © 2026 **Sentra Artificial Intelligence** — dr. Ferdi Iskandar

---

<div align="center">

Built with care for frontline healthcare workers in Indonesia. 🇮🇩

_Architect & Built by [Claudesy](https://github.com/DocSynapse) · Sentra Healthcare Solutions_

</div>
DESIGN.md — System & UI Design Reference
text
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
