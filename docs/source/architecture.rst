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
The backend is a RESTful API service built using **Dart Frog**. It acts as the secure intermediary between the Flutter frontend, the PostgreSQL database, and external APIs (like Google Places).

* **Framework & Routing**: Uses **dart_frog** for lightweight and fast route handling.
* **Database Communication**: Connects directly to the PostgreSQL database using the **postgres** driver package. Database operations are abstracted into a Repository layer (e.g., separating SQL queries from route logic).
* **Authentication & Security**: Implements secure, stateless sessions. User passwords are encrypted using **bcrypt**, and authentication is managed via JSON Web Tokens (**dart_jsonwebtoken**). Custom middleware validates these tokens on protected routes.
* **Secret Management**: Loads environment configurations (database credentials, JWT secrets, Google API keys) from a `.env` file using the **dotenv** package.
* **External Integrations**: Uses the **http** package to make server-to-server calls. Specifically, it proxies requests to the Google Places API, ensuring the Google API key is never exposed to the client frontend.

Infrastructure Notes
--------------------

- `docker-compose.yml` configures the frontend, backend, and PostgreSQL services.
- `docker-compose.override.yml` is used for local overrides, such as development-specific settings.
- `.env.example` contains sample environment variables used by Docker and the app.
