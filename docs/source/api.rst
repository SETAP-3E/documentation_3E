API Reference
=============

This section documents the REST API exposed by the Dart Frog backend. All requests generally accept and return JSON payloads.

Base URL: ``http://localhost:8080`` (or your production domain).

Authentication (`/auth`)
------------------------

All endpoints in the ``/auth`` path are public and do not require prior authorization.

.. http:post:: /auth/signup

   Creates a new user account and returns a JWT token for immediate login.

   **Request Body:**

   .. code-block:: json

      {
        "username": "user123",
        "password": "strongPassword!"
      }

   **Response (201 Created):**

   .. code-block:: json

      {
        "token": "eyJhbGciOiJIUzI1NiIsIn...",
        "user_id": 1,
        "username": "user123"
      }

   **Error Responses:**
   * ``400 Bad Request``: Username or password missing, or validation failed (e.g., password < 8 chars).
   * ``409 Conflict``: Username already taken.

.. http:post:: /auth/login

   Validates user credentials and returns a JWT session token.

   **Request Body:**

   .. code-block:: json

      {
        "username": "user123",
        "password": "strongPassword!"
      }

   **Response (200 OK):**

   .. code-block:: json

      {
        "token": "eyJhbGciOiJIUzI1NiIsIn...",
        "user_id": 1,
        "username": "user123"
      }

   **Error Responses:**
   * ``400 Bad Request``: Username or password not provided.
   * ``401 Unauthorized``: Invalid username or password generic error.
