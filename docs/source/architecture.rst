Architecture
============

Project Structure
-----------------

The repository is organized into frontend, backend, and infrastructure components.

.. code-block:: text

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

Architecture Overview
---------------------

Frontend Architecture (Flutter)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The frontend is built with **Flutter** (targeting Web). It follows a layered approach focusing on the separation of presentation, business logic, and data access.

* **State Management**: Utilizes the **BLoC (Business Logic Component)** pattern via the ``flutter_bloc`` package to decouple UI from business rules.
* **Routing**: Managed by **go_router** for declarative navigation and route guards (e.g., redirecting unauthenticated users to the login screen).
* **Networking**: HTTP requests to the backend are managed by the **Dio** networking package, handling token interceptors and serialization.
* **Storage**: secure storage of session data and JWT tokens using **flutter_secure_storage**.
* **Visuals & Maps**: Uses **fl_chart** for rendering financial data and **google_maps_flutter** for transaction location plotting.

Backend Architecture (Dart Frog)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* [Backend details placeholder]

Infrastructure Notes
--------------------

- `docker-compose.yml` configures the frontend, backend, and PostgreSQL services.
- `docker-compose.override.yml` is used for local overrides, such as development-specific settings.
- `.env.example` contains sample environment variables used by Docker and the app.
