Installation
=============

Prerequisites
-------------

Before running the application, ensure you have the following installed:

- `Docker Desktop <https://www.docker.com/products/docker-desktop/>`_ **or** `Colima <https://github.com/abiosoft/colima>`_ (macOS)
- `Docker Compose <https://docs.docker.com/compose/>`_ (v2 — ``docker compose`` subcommand)
- `Flutter SDK <https://docs.flutter.dev/get-started/install>`_ (Required for local frontend development)
- `Dart SDK <https://dart.dev/get-dart>`_ (Required for local backend development using Dart Frog)

Clone the Repository
--------------------

.. code-block:: console

   git clone https://github.com/CameronGibbons/Budgetting-App.git
   cd Budgetting-App

Environment Configuration
--------------------------

Create your environment file:

.. code-block:: console

   cp .env.example .env

Open ``.env`` and set secure values:

.. code-block:: text

   # Database Credentials
   POSTGRES_DB=budgetting
   POSTGRES_USER=budgetting_user
   POSTGRES_PASSWORD=your_secure_password_here

   # Backend Security
   JWT_SECRET=your_long_random_secret_here
   PORT=8080

   # Frontend configuration
   API_BASE_URL=http://localhost:8080

   # External APIs
   GOOGLE_PLACES_API_KEY=your_google_api_key_here

.. note::
   To enable location tagging, you must acquire a Google Places API key from the `Google Cloud Console <https://console.cloud.google.com/>`_ and place it in the ``GOOGLE_PLACES_API_KEY`` field.

.. warning::

   Never commit ``.env`` to version control. It is already listed in ``.gitignore``.

Running the Application (Docker Compose)
----------------------------------------

Once your ``.env`` is configured, the simplest way to run the entire backend, frontend, and database locally is via Docker Compose:

.. code-block:: console

   docker compose up --build -d

This command spins up the following services:
* ``postgres`` container running on port ``5433``
* ``backend`` container running on port ``8080``
* ``frontend`` container acting as a web server on port ``80``

You can view the live application at ``http://localhost:80`` (or via your specified proxy limits).

To stop the environment:

.. code-block:: console

   docker compose down

To completely wipe the database volume and start fresh, run:

.. code-block:: console

   docker compose down -v

macOS — Docker without Docker Desktop
-------------------------------------

If you choose to use `Colima <https://github.com/abiosoft/colima>`_ instead of Docker Desktop on a Mac:

.. code-block:: console

   # Install Colima, Docker, and Compose plugin
   brew install colima docker docker-compose

   # Configure the docker compose plugin
   mkdir -p ~/.docker

Open or create ``~/.docker/config.json`` and add:

.. code-block:: json

   {
     "cliPluginsExtraDirs": ["/opt/homebrew/lib/docker/cli-plugins"]
   }

Then start the VM and run the stack:

.. code-block:: console

   # Start the VM
   colima start

   # Then run the project as normal
   docker compose up --build -d

Initializing the Database
-------------------------

Before the application functions properly, the database must be given its schema and optional seed data. 
Ensure the Docker Compose stack is already running (specifically the ``postgres`` container), then run the following commands from the root directory of the repository:

1. **Apply the Schema** (Creates tables):

.. code-block:: console

   cat database/schema.sql | docker compose exec -T postgres psql -U budgetting_user -d budgetting

2. **Seed Local Data** (Optional: Adds test users and initial mocked transactions):

.. code-block:: console

   cat database/seeds/dev_seed.sql | docker compose exec -T postgres psql -U budgetting_user -d budgetting
