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

Accounts (`/accounts`)
----------------------

All endpoints in the ``/accounts`` path require a valid Bearer JWT token in the ``Authorization`` header.

.. http:get:: /accounts

   Retrieves a list of all accounts belonging to the authenticated user. Includes dynamically computed spend amounts for the current month and week.

   **Response (200 OK):**

   .. code-block:: json

      [
        {
          "id": "uuid-here",
          "user_id": "user-uuid",
          "name": "Main Checking",
          "account_type": "current",
          "balance": 2500.00,
          "monthly_budget": 1000.00,
          "monthly_spent": 350.25,
          "weekly_spent": 120.00,
          "accent_color": 4278190080
        }
      ]

.. http:post:: /accounts

   Creates a new financial account/budget for the user.

   **Request Body:**

   .. code-block:: json

      {
        "name": "Holiday Fund",
        "account_type": "savings",
        "balance": 500.0,
        "monthly_budget": 100.0,
        "accent_color": 4280391411
      }

   *   ``account_type`` must be one of: ``"current"``, ``"savings"``, or ``"joint"``.

   **Response (201 Created):**

   .. code-block:: json

      {
         "id": "new-uuid-here",
         "user_id": "user-uuid",
         "name": "Holiday Fund",
         "account_type": "savings",
         "balance": 500.0,
         "monthly_budget": 100.0,
         "monthly_spent": 0.0,
         "weekly_spent": 0.0,
         "accent_color": 4280391411
      }

   **Error Responses:**
   * ``400 Bad Request``: Missing required fields, invalid account type, or negative balance/monthly budget.

Transactions (`/transactions`)
------------------------------

All endpoints require a valid Bearer JWT token.

.. http:get:: /transactions

   Retrieves a list of transactions for the authenticated user.

   **Query Parameters:**
   * ``account_id`` (optional): Filter to a specific account.
   * ``limit`` (optional): Number of records to return.
   * ``offset`` (optional): Pagination offset.

   **Response (200 OK):**

   .. code-block:: json

      [
        {
          "id": "trans-uuid",
          "user_id": "user-uuid",
          "account_id": "acc-uuid",
          "category_id": "cat-uuid",
          "category_name": "Groceries",
          "account_name": "Main Checking",
          "amount": 25.50,
          "place_name": "Tesco",
          "transaction_date": "2024-05-15",
          "latitude": 51.5074,
          "longitude": -0.1278
        }
      ]

.. http:post:: /transactions

   Creates a new financial transaction.

   **Request Body:**

   .. code-block:: json

      {
        "account_id": "acc-uuid",
        "amount": 25.50,
        "transaction_date": "2024-05-15",
        "place_name": "Tesco",
        "latitude": 51.5074,
        "longitude": -0.1278,
        "category_id": "cat-uuid"
      }

   *   ``category_id`` OR ``new_category_name`` must be provided. If the latter is used, the backend creates a custom category dynamically.
   *   ``latitude`` and ``longitude`` must be valid coordinates or entirely omitted.

   **Response (201 Created):**
   Returns the created transaction object.

Budgets (`/budgets`)
--------------------

All endpoints require a valid Bearer JWT token.

.. http:get:: /budgets

   Retrieves the user's budget performance against actual spending for a given calendar month.

   **Query Parameters:**
   * ``year`` (required): e.g., ``2024``
   * ``month`` (required): e.g., ``5``

   **Response (200 OK):**

   .. code-block:: json

      [
        {
          "category_id": "cat-uuid",
          "name": "Entertainment",
          "colour_value": 4278190080,
          "goal_amount": 100.0,
          "spent_amount": 65.50,
          "percentage": 65.5
        }
      ]

.. http:post:: /budgets

   Creates or updates a monthly budget goal for a specific category.

   **Query/Body payload properties:** Requires ``category_id``, ``year``, ``month``, and ``goal_amount``.

.. http:delete:: /budgets
   
   Removes a budget goal. Requires ``category_id``, ``year``, and ``month`` query parameters.

Categories (`/categories`)
--------------------------

All endpoints require a valid Bearer JWT token.

.. http:get:: /categories

   Retrieves all spending categories available to the authenticated user.
   This returns predefined systemic categories plus any custom categories created by the user, ordered predefined-first then alphabetically.

   **Response (200 OK):**

   .. code-block:: json

      [
        {
          "id": "cat-uuid",
          "name": "Groceries",
          "icon": "shopping_cart",
          "colour_value": 4283120984,
          "is_predefined": true
        }
      ]

Places (`/places`)
------------------

The Places endpoints act as a proxy for the Google Places API to avoid exposing the Google API Key to the frontend and to prevent CORS issues. All calls require a valid Bearer JWT token.

.. http:get:: /places/autocomplete

   Proxies the query to the Google Places Autocomplete API. Returns a trimmed list of predictions scoped to the `GB` region.

   **Query Parameters:**
   * ``q`` (required): The search input string.

   **Response (200 OK):**

   .. code-block:: json

      {
        "predictions": [
          {
            "place_id": "ChIJ...id",
            "description": "Tesco Superstore, Fratton Way, Portsmouth, UK"
          }
        ]
      }

.. http:get:: /places/details

   Proxies to the Google Places Details API, fetching only the place's ``name`` and ``geometry`` coordinates to minimize billing costs.

   **Query Parameters:**
   * ``place_id`` (required): The Google Place ID obtained from the autocomplete endpoint.

   **Response (200 OK):**

   .. code-block:: json

      {
        "name": "Tesco Superstore",
        "latitude": 50.7963,
        "longitude": -1.0664
      }
