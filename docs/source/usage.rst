Usage Guide
===========

This guide provides a user-centric walkthrough for the main features of the Budgeting App frontend.

User Registration and Login
---------------------------

New users can create an account, and existing users can log in to access their financial data.

1.  **Registration**:
    *   Navigate to the "Sign Up" page.
    *   Enter a unique username, a valid email, and a secure password.
    *   Upon successful registration, you will be automatically logged in and redirected to the main dashboard.

2.  **Login**:
    *   Navigate to the "Login" page.
    *   Enter your email and password.
    *   The application will fetch a JWT token from the backend and store it securely in the browser's local storage for session management.
    *   You will be redirected to the dashboard.

Creating Accounts and Budgets
-----------------------------

Once logged in, you can set up your financial accounts and define monthly budgets.

1.  **Create a Financial Account**:
    *   From the dashboard, navigate to the "Accounts" section.
    *   Click "Add Account".
    *   Provide a name (e.g., "Main Checking"), select an account type (e.g., "checking"), and set the initial balance.
    *   You can also set an optional overall monthly spending budget for this account.

2.  **Set a Category Budget**:
    *   Navigate to the "Budgets" page.
    *   Select a month and year.
    *   Click on a spending category (e.g., "Groceries").
    *   Enter your desired budget amount for that category for the selected month. This will now appear on your budget tracking view.

Logging Transactions
--------------------

Track your day-to-day spending by logging transactions.

1.  From the main navigation, click the "Add Transaction" button.
2.  Fill in the transaction details:
    *   **Amount**: The cost of the transaction.
    *   **Date**: The date the transaction occurred.
    *   **Account**: Select which of your financial accounts to use.
    *   **Category**: Assign a spending category. You can select a predefined one or create a new custom category on the fly by typing a new name.
3.  **Location (Optional)**:
    *   Use the search bar to find a place via the Google Places API (e.g., "Tesco").
    *   Select the correct location from the autocomplete suggestions.
    *   The transaction will be tagged with the place name and its geographic coordinates.
    *   Alternatively, you can manually drop a pin on the map.

