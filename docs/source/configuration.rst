Configuration
=============

Environment Variables
---------------------

The application uses the following environment variables for database connectivity,
secrets, and API configuration.

.. list-table:: Environment variables
   :header-rows: 1

   * - Variable
     - Description
     - Default
   * - ``POSTGRES_DB``
     - Database name
     - ``budgetting``
   * - ``POSTGRES_USER``
     - Database user
     - ``budgetting_user``
   * - ``POSTGRES_PASSWORD``
     - Database password
     - *(must be set)*
   * - ``JWT_SECRET``
     - Secret key for signing JWT tokens
     - *(must be set)*
   * - ``PORT``
     - Backend server port
     - ``8080``
   * - ``API_BASE_URL``
     - Base URL the frontend uses to call the backend API
     - ``http://localhost:8080``

Notes
-----

- The values in ``.env.example`` are placeholders and should be customized for your
  environment.
- Never commit a file containing secrets such as ``POSTGRES_PASSWORD`` or ``JWT_SECRET``
  to version control.
