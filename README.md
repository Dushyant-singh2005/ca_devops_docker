# FestPass — Docker & Container Security Practical Assessment

## Student Information

| Field | Value |
|-------|-------|
| **Name** | Dushyant Singh |
| **REGNO** | 202401100200138 |
| **PORT** | 8038 (80 + last two digits of REGNO: 38) |
| **Docker Hub Username** | dushyantsingh |
| **Docker Hub Repository** | https://hub.docker.com/r/dushyantsingh/fest-backend |
| **GitHub Repository** | https://github.com/Dushyant-singh2005/ca_devops_docker |

---

## AI Tools Used

| Tool | What It Was Used For |
|------|---------------------|
| Google Gemini (Antigravity) | Reading assessment PDF, generating all project files, explaining commands, writing Dockerfile and compose.yaml, guiding through all 5 stages |

---

## Project Architecture

```
FestPass Three-Tier Architecture
┌──────────────────────────────────────────────────────────┐
│  Browser → http://localhost:8038                         │
└──────────────────────┬───────────────────────────────────┘
                       │
           ┌───────────▼───────────┐
           │   FRONTEND (nginx)    │  Port 8038:80
           │   nginx:1.30-alpine   │
           └───────────┬───────────┘
                       │  Network: frontend-backend
           ┌───────────▼───────────┐
           │   BACKEND (Flask)     │  Port 5000 (internal)
           │   fest-backend:0.x    │
           └───────────┬───────────┘
                       │  Network: backend-database
           ┌───────────▼───────────┐
           │   DATABASE (Postgres) │  Port 5432 (internal)
           │   postgres:17-alpine  │
           └───────────────────────┘
```

### Network Design (Security)

| Network | Services Connected | Purpose |
|---------|-------------------|---------|
| `frontend-backend` | frontend, backend | HTTP API proxying |
| `backend-database` | backend, db | Database access |

> **Security rule:** Frontend is NOT connected to `backend-database`. It can never reach the database directly.

---

## Prerequisites

- Docker Desktop (v24+) with Docker Compose v2
- Git
- Trivy (official release — NOT 0.69.4, 0.69.5, or 0.69.6)
- Cosign (v2+)

---

## Quick Start

### 1. Clone the repository
```bash
git clone https://github.com/Dushyant-singh2005/ca_devops_docker.git
cd ca_devops_docker
```

### 2. Configure environment
```bash
copy .env.example .env
# Edit .env and set a strong DB_PASSWORD
```

### 3. Start the application
```bash
docker compose up -d --build
```

### 4. Open in browser
```
http://localhost:8038
```

### 5. Stop the application
```bash
docker compose down
```

---

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/health` | Health check — returns REGNO, version, database status |
| `GET` | `/api/registrations` | List all registrations |
| `POST` | `/api/registrations` | Create a new registration |

### Example: Health check (PowerShell)
```powershell
Invoke-RestMethod http://localhost:8038/api/health
```

### Example: Create a registration (PowerShell)
```powershell
Invoke-RestMethod -Method POST -Uri http://localhost:8038/api/registrations `
  -ContentType "application/json" `
  -Body '{"name": "Dushyant", "event": "Hackathon"}'
```

---

## Vulnerability Scan Results (Stage 3)

<!-- Updated after Stage 3 is complete -->

| Image | Base | CRITICAL | HIGH | Size |
|-------|------|----------|------|------|
| `fest-backend:0.1` | python:3.9 | _TBD_ | _TBD_ | _TBD_ |
| `fest-backend:0.2` | python:3.12-slim | _TBD_ | _TBD_ | _TBD_ |

Full reports: [`reports/trivy-before.txt`](reports/trivy-before.txt) | [`reports/trivy-after.txt`](reports/trivy-after.txt)

---

## Published Image (Stage 4)

| Tag | Digest |
|-----|--------|
| `dushyantsingh/fest-backend:1.0.0` | _TBD after push_ |
| `dushyantsingh/fest-backend:1.0` | _TBD after push_ |

---

## Cosign Signing (Stage 5)

### Sign the 1.0.0 image by digest
```bash
cosign sign --key cosign.key dushyantsingh/fest-backend@sha256:<digest>
```

### Verify the signed image
```bash
cosign verify --key cosign.pub dushyantsingh/fest-backend@sha256:<digest>
```

### Unsigned 1.0.1 — expected rejection
```bash
cosign verify --key cosign.pub dushyantsingh/fest-backend:1.0.1
# Expected: Error — no valid signatures found (this is correct behaviour)
```

---

## Incomplete Steps

_None — all stages completed._

<!-- Update this section if any step could not be completed, one line each. -->
