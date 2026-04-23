Quick Start with Docker Compose
================================

The recommended way to run the entire application stack is with Docker Compose.

Running the Full Stack
----------------------

Once you've completed the :doc:`installation` steps, start all services with:

.. code-block:: console

   docker compose up --build

This starts three services:

| Service    | URL                      |
|------------|--------------------------|
| Frontend   | http://localhost:80      |
| Backend    | http://localhost:8080    |
| PostgreSQL | localhost:5432           |

Running in the Background
--------------------------

To run the services in detached mode (background):

.. code-block:: console

   docker compose up --build -d

Stopping the Services
---------------------

To stop all services:

.. code-block:: console

   docker compose down

To also remove the database volume (this **wipes all data**):

.. code-block:: console

   docker compose down -v

.. warning::

   Running ``docker compose down -v`` will permanently delete your database. Use with caution in production environments.
