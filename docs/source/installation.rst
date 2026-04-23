Installation
=============

Prerequisites
-------------

Before running the application, ensure you have the following installed:

- `Docker Desktop <https://www.docker.com/products/docker-desktop/>`_ **or** `Colima <https://github.com/abiosoft/colima>`_ (macOS)
- `Docker Compose <https://docs.docker.com/compose/>`_ (v2 — ``docker compose`` subcommand)
- `Flutter SDK <https://docs.flutter.dev/get-started/install>`_ (for local development only)

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

   POSTGRES_DB=budgetting
   POSTGRES_USER=budgetting_user
   POSTGRES_PASSWORD=your_secure_password_here

   JWT_SECRET=your_long_random_secret_here
   PORT=8080

   API_BASE_URL=http://localhost:8080

.. warning::

   Never commit ``.env`` to version control. It is already listed in ``.gitignore``.
