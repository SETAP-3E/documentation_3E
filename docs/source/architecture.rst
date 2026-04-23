Architecture
============

Project Structure
-----------------

The repository is organized into frontend, backend, and infrastructure components.

```
Budgetting-App/
├── frontend/           # Flutter web application
│   ├── lib/
│   │   ├── core/       # Theme, router, utilities
│   │   └── features/   # Feature modules (dashboard, auth, etc.)
│   └── Dockerfile
├── backend/            # Dart Frog REST API
│   ├── routes/         # API route handlers
│   ├── lib/            # Models, services, repositories
│   └── Dockerfile
├── docker-compose.yml
├── docker-compose.override.yml  # Local dev overrides
└── .env.example
```

Architecture Overview
---------------------

- Frontend: Flutter web app that communicates with the backend API.
- Backend: Dart Frog REST API serving budget and authentication endpoints.
- Database: PostgreSQL stores user accounts, budgets, transactions, and settings.
- Infra: Docker Compose defines the full stack for development and local testing.

Infrastructure Notes
--------------------

- `docker-compose.yml` configures the frontend, backend, and PostgreSQL services.
- `docker-compose.override.yml` is used for local overrides, such as development-specific settings.
- `.env.example` contains sample environment variables used by Docker and the app.
