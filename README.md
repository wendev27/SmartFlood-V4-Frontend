# SmartFlood V4 — Separated Full-Stack Application

SmartFlood is an AI-assisted flood monitoring and disaster-relief coordination platform designed to support flood monitoring, resident information management, and relief allocation workflows.

This repository contains the separated SmartFlood V4 application, organized into a Next.js frontend, a backend-owned API service, and a Python FastAPI AI service. The separation establishes clearer service boundaries while aiming to preserve existing API contracts, authentication behavior, database integrations, and application workflows.

## Architecture

```text
Browser
   |
   v
Frontend
Next.js · React · TypeScript
   |
   | /api/* requests
   | NEXT_PUBLIC_API_URL rewrite
   v
Backend API
Next.js Route Handlers
   |
   +----> Supabase / PostgreSQL
   |
   +----> MongoDB Atlas
   |
   +----> AI Service
          Python · FastAPI
```

The frontend owns presentation and user interactions. The API service owns server-side application logic, authentication/session handling, authorization checks, and privileged data access. The AI service handles AI-assisted recommendation workflows.

## Key Features

- Flood monitoring dashboards, sensor maps, and flood history.
- Resident and household information management.
- Sensor monitoring and related administrative workflows.
- AI-assisted relief recommendations and allocation.
- Relief inventory, campaigns, distribution, and verification workflows.
- Notifications, reporting, and role-based navigation.

Feature availability depends on the configured backend services, user permissions, and available data.

## Technology Stack

| Component | Technologies |
|---|---|
| Frontend | Next.js 16, React 19, TypeScript |
| Backend API | Next.js Route Handlers, TypeScript |
| AI service | Python, FastAPI |
| Databases | Supabase / PostgreSQL and MongoDB |
| Containerization | Docker |
| Cloud deployment configuration | Google Cloud Build and Cloud Run |

## Repository Structure

```text
.
├── Frontend/           # Next.js user interface
├── Backend/
│   ├── api/            # API routes and server-side logic
│   └── ai/             # FastAPI AI service
├── supabase/           # Database migration references
├── cloudbuild.yaml     # API image build and deployment configuration
└── README.md
```

## Local Development

Use a supported Node.js version and the package manager corresponding to each service's lockfile.

### 1. Frontend

```bash
cd Frontend
npm install
npm run dev
```

The development server normally runs at `http://localhost:3000`.

### 2. Backend API

In another terminal:

```bash
cd Backend/api
npm install
npm run dev
```

The current development script uses port `5000`.

### 3. AI service

In another terminal:

```bash
cd Backend/ai
python3 -m venv .venv
.venv/bin/python -m pip install -r requirements.txt
.venv/bin/python -m uvicorn app.main:app --reload --port 8000
```

Configure the required environment variables for each service before testing data-backed workflows.

## Environment Configuration

Use the tracked environment example files as the source of truth:

- `Frontend/.env.example`
- `Backend/api/.env.example`
- `Backend/ai/.env.example`

The frontend's `NEXT_PUBLIC_API_URL` determines the backend API target used by its `/api/*` rewrite. Its default configuration targets the local API at port `5000`.

Never place private database credentials, Supabase service-role keys, session-signing secrets, or other server-side credentials in browser-visible environment variables.

## Deployment

The repository includes a `cloudbuild.yaml` configuration for building the API Docker image and deploying the `smartflood-api` service to Google Cloud Run in `asia-southeast1`, with port `5000` configured.

The presence of deployment configuration does not establish that the latest deployment is healthy. Confirm the active Cloud Run revision, application startup, health endpoint, database connectivity, and critical API workflows before reporting production readiness.

## Security Considerations

- Enforce authentication and authorization on the backend for protected operations.
- Validate input and enforce appropriate data-access scope on every protected request.
- Keep privileged credentials exclusively in server-side configuration or a secret manager.
- Restrict CORS to intended application origins.
- Keep real environment files, access tokens, private keys, and production credentials out of Git.

## Related Repositories

- [SmartFlood V4 Cloud Run Backend](https://github.com/wendev27/SmartFlood-V4-CloudRun-Backend)
- [SmartFlood V5 AWS Migration](https://github.com/wendev27/Smart-Flood-V5-AWS)

## Project Status

SmartFlood V4 represents the separated application and its service boundaries. Application validation, cloud deployment health, and production readiness must be assessed independently. Do not treat configured infrastructure or successful source checks as proof that all services and workflows have been verified in production.
