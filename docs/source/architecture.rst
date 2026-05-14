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

Database & Infrastructure
~~~~~~~~~~~~~~~~~~~~~~~~~
The app utilizes **Docker** and **Docker Compose** to orchestrate the entire environment securely and reliably.

* **PostgreSQL Database**: Data is stored using a structured relational model residing in a `postgres:16-alpine` Docker container. The data is persisted across restarts using Docker volumes (`postgres_data`). Health checks ensure the database is fully initialized before the backend attempts to connect.
* **Component Containerization**:
  * The **Backend** container awaits the database and is injected with vital environment variables (database URLs, JWT secrets, and Google API keys).
  * The **Frontend** container serves the compiled Flutter web artifacts.
* **Local Overrides**: A `docker-compose.override.yml` facilitates local development tweaks, like mapping the database port to `5433` on the host machine.
* **Environment Configuration**: All secrets and environment-specific configs are managed via a `.env` file (with `.env.example` acting as the template).

External Integrations: Google Maps & Places
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
A core feature of the Budgeting app is location-aware transaction tagging. The architecture ensures both a seamless UI and secure API key management:

* **Frontend Maps UI**: The Flutter app leverages the `google_maps_flutter` package to render an interactive map. Users can select locations or search for specific venues.
* **Secure Places API Proxy**: To prevent the sensitive `GOOGLE_PLACES_API_KEY` from being exposed in the public web frontend, the system proxies Google Places requests through the backend.
  1. The Flutter client sends a search request (e.g., query text) to the Dart Frog backend (`/places/autocomplete` or `/places/details`).
  2. The Dart Frog backend appends the secure API key and forwards the request to the official Google Places API.
  3. The backend receives Google's response and relays the cleaned location data back to the frontend.
