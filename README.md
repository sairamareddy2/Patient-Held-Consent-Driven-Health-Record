# PS3 Patient-Held Consent-Driven Health Record

**Team NSS · VNRVJIET · VJH 2k26**

> **Core Architectural Principle:**  
> *"Identification ≠ Authorization — Your health record. Your consent. Your control."*

A mobile-first, cloud-hosted foundation for a patient-held longitudinal health record. A patient reference ID identifies a record; only explicit, purpose- and time-scoped consent authorizes healthcare provider access.

---

## Live Deployments & Demo

| Resource | URL | Description |
| :--- | :--- | :--- |
| **Live Deployed Prototype** | [patient-health-record-git-main...vercel.app](https://patient-health-record-git-main-shivateja2109s-projects.vercel.app) | Complete Web Application (Vercel) |
| **Backend REST API** | [ps3-health-record-api.onrender.com](https://ps3-health-record-api.onrender.com) | FastAPI Core Service (Render) |
| **Interactive API Documentation** | [ps3-health-record-api.onrender.com/docs](https://ps3-health-record-api.onrender.com/docs) | Swagger / OpenAPI 3.1 UI |
| **Service Health Check** | [ps3-health-record-api.onrender.com/health/live](https://ps3-health-record-api.onrender.com/health/live) | Live Probe Endpoint |
| **Full Demonstration Video** | [`ps3_live_demonstration.mp4`](./ps3_live_demonstration.mp4) | 1080p Screen Recording with Audio Narration (3m 41s) |

---

## Implementation Phases

### Phase 0: Hosted Cloud Foundation
- Monorepo structure: Next.js frontend (`apps/web`), FastAPI backend (`apps/api`), shared types (`packages/shared-types`).
- Hosted Supabase PostgreSQL with Alembic migrations.
- Hosted Supabase private S3 Storage for health records.
- Redis integration for caching and session state.
- Automated CI/CD deployments on Vercel and Render.

### Phase 1: Patient-Owned Records & Provider Profiles
- Authentication with JWT tokens and role-based access control (`patient`, `provider`, `caregiver`).
- Patient demographic profiles and automatic sovereign **Reference ID** generation (`PHR-XXXXXXXXXX`).
- Healthcare provider profiles with clinical specialty and verification status.
- Patient-owned health record CRUD (visits, diagnoses, medications, labs, vaccinations).
- Longitudinal health history timeline views.
- Secure, short-lived presigned URLs for document upload/download.
- Initial append-only audit logging for profile, record, and document events.

### Phase 2: QR Identification & Audited Consent Core
- **Identification vs. Authorization:** QR code and Reference ID identify the patient's record without granting inherent read access.
- **Provider Access Requests:** Providers submit time-bounded (`15m`, `1h`, `4h`, `24h`, `7d`) and purpose-scoped (`treatment`, `care_coordination`, `diagnostic_review`, `emergency_break_glass`) requests.
- **Real Security Enforcement:** Attempts to view patient records prior to explicit approval strictly return **HTTP 403 Forbidden**.
- **Patient Sovereignty & Approval:** Patients review pending provider credentials, facility, and clinical purpose before granting time-bounded consent.
- **Active Countdown Timers:** Providers view authorized medical timelines with real-time expiration countdown indicators.
- **Immediate Sovereign Revocation:** Patients can instantly terminate access at any time; provider access is blocked immediately on the subsequent read.
- **Append-Only Audit Trail:** Comprehensive event capture (`consent_requested`, `consent_granted`, `records_viewed_by_provider`, `consent_revoked`).

---

## Cloud Architecture

- **Supabase PostgreSQL & Private Storage:** Primary relational database with automated schema migrations and private file buckets.
- **Upstash Redis:** Ephemeral state and token revocation.
- **Vercel:** Edge-optimized Next.js web application.
- **Render:** Dockerized FastAPI service with automated startup migrations.

---

## Repository Structure

```
├── apps/
│   ├── api/                  # FastAPI backend service
│   │   ├── migrations/       # Alembic database migrations (0001, 0002, 0003)
│   │   ├── src/
│   │   │   ├── models/       # SQLAlchemy models (User, Patient, Provider, Record, Consent, Audit)
│   │   │   ├── routes/       # API endpoints (auth, patients, providers, records, consents, audit)
│   │   │   ├── services/     # Business logic & consent validation middleware
│   │   │   └── worker/       # Background workers (consent expiry sweeper)
│   └── web/                  # Next.js 15 frontend application
│       ├── app/
│       │   ├── (patient)/    # Patient portal (dashboard, timeline, documents, consents)
│       │   ├── (provider)/   # Provider portal (dashboard, request-access, patient view)
│       │   └── (public)/     # Landing page, auth forms, QR scan viewer
├── docs/                     # Architecture specifications & threat models
├── ps3_live_demonstration.mp4 # Full HD 1080p live demonstration video
└── render.yaml               # Infrastructure-as-code for Render deployment
```

---

## License & Usage

Developed for **VJH 2k26**. Use synthetic demo data only until an appropriate clinical compliance and privacy review is complete.
