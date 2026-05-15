.. _database-schema:

Database Schema
===============

The application uses a PostgreSQL database, orchestrated via Docker. The schema is defined through a series of migration files located in ``database/migrations/``.

Users (`users`)
---------------

Stores user authentication and profile information.

.. list-table::
   :header-rows: 1

   * - Column
     - Type
     - Description
   * - ``id``
     - ``UUID``
     - Primary Key.
   * - ``username``
     - ``VARCHAR(255)``
     - Unique username for display.
   * - ``email``
     - ``VARCHAR(255)``
     - Unique email for login.
   * - ``hashed_password``
     - ``VARCHAR(255)``
     - Bcrypt-hashed password.
   * - ``created_at``
     - ``TIMESTAMPTZ``
     - Timestamp of user creation.

Accounts (`accounts`)
---------------------

Represents a user's financial account (e.g., checking, savings).

.. list-table::
   :header-rows: 1

   * - Column
     - Type
     - Description
   * - ``id``
     - ``UUID``
     - Primary Key.
   * - ``user_id``
     - ``UUID``
     - Foreign Key to ``users(id)``.
   * - ``name``
     - ``VARCHAR(255)``
     - Account name (e.g., "Main Checking").
   * - ``type``
     - ``VARCHAR(50)``
     - Account type (e.g., "checking", "savings").
   * - ``balance``
     - ``DECIMAL(10, 2)``
     - Current account balance.
   * - ``monthly_budget``
     - ``DECIMAL(10, 2)``
     - Optional overall monthly budget for the account.
   * - ``created_at``
     - ``TIMESTAMPTZ``
     - Timestamp of account creation.

Categories (`categories`)
-------------------------

Stores spending categories, both predefined and user-created.

.. list-table::
   :header-rows: 1

   * - Column
     - Type
     - Description
   * - ``id``
     - ``UUID``
     - Primary Key.
   * - ``user_id``
     - ``UUID``
     - Foreign Key to ``users(id)``. Null for predefined categories.
   * - ``name``
     - ``VARCHAR(255)``
     - Category name (e.g., "Groceries").
   * - ``icon``
     - ``VARCHAR(255)``
     - Material icon name for the UI.
   * - ``colour_value``
     - ``INTEGER``
     - ARGB integer for UI colour.
   * - ``is_predefined``
     - ``BOOLEAN``
     - True if it's a system-wide category.

Transactions (`transactions`)
-----------------------------

Logs individual financial transactions.

.. list-table::
   :header-rows: 1

   * - Column
     - Type
     - Description
   * - ``id``
     - ``UUID``
     - Primary Key.
   * - ``user_id``
     - ``UUID``
     - Foreign Key to ``users(id)``.
   * - ``account_id``
     - ``UUID``
     - Foreign Key to ``accounts(id)``.
   * - ``category_id``
     - ``UUID``
     - Foreign Key to ``categories(id)``.
   * - ``amount``
     - ``DECIMAL(10, 2)``
     - Transaction amount.
   * - ``transaction_date``
     - ``DATE``
     - Date of the transaction.
   * - ``place_name``
     - ``VARCHAR(255)``
     - Name of the place/merchant.
   * - ``latitude``
     - ``DOUBLE PRECISION``
     - Optional latitude of the transaction.
   * - ``longitude``
     - ``DOUBLE PRECISION``
     - Optional longitude of the transaction.
   * - ``created_at``
     - ``TIMESTAMPTZ``
     - Timestamp of transaction logging.

Budgets (`budgets`)
-------------------

Stores monthly budget goals for specific categories.

.. list-table::
   :header-rows: 1

   * - Column
     - Type
     - Description
   * - ``id``
     - ``UUID``
     - Primary Key.
   * - ``user_id``
     - ``UUID``
     - Foreign Key to ``users(id)``.
   * - ``category_id``
     - ``UUID``
     - Foreign Key to ``categories(id)``.
   * - ``goal_amount``
     - ``DECIMAL(10, 2)``
     - The budget goal for the month.
   * - ``month``
     - ``INTEGER``
     - The month of the budget (1-12).
   * - ``year``
     - ``INTEGER``
     - The year of the budget.
   * - ``created_at``
     - ``TIMESTAMPTZ``
     - Timestamp of budget creation.

Monthly Balances (`monthly_balances`)
-------------------------------------

A summary table to track account balances over time.

.. list-table::
   :header-rows: 1

   * - Column
     - Type
     - Description
   * - ``id``
     - ``UUID``
     - Primary Key.
   * - ``account_id``
     - ``UUID``
     - Foreign Key to ``accounts(id)``.
   * - ``balance``
     - ``DECIMAL(10, 2)``
     - Balance at the end of the month.
   * - ``month``
     - ``INTEGER``
     - The month (1-12).
   * - ``year``
     - ``INTEGER``
     - The year.
