Testing and Quality Assurance
===========================

This section outlines the procedures for running automated tests for both the frontend and backend components of the application.

Frontend (Flutter)
------------------

The Flutter frontend includes a suite of widget and unit tests to ensure UI components and business logic behave as expected.

**Running Tests:**

To run all frontend tests, navigate to the ``frontend`` directory and execute:

.. code-block:: console

   $ flutter test

**Generating Code Coverage:**

To generate a test coverage report, run:

.. code-block:: console

   $ flutter test --coverage

This will create a ``coverage/lcov.info`` file. You can visualize this report by installing ``lcov`` and running the following commands:

.. code-block:: console

   # Install lcov (on macOS with Homebrew)
   $ brew install lcov

   # Generate HTML report
   $ genhtml coverage/lcov.info -o coverage/html

Then, open ``coverage/html/index.html`` in a web browser to view the detailed report.

Backend (Dart Frog)
-------------------

The Dart Frog backend has its own set of unit and integration tests for routes, middleware, and service logic.

**Running Tests:**

To run all backend tests, navigate to the ``backend`` directory and use the standard Dart test runner:

.. code-block:: console

   $ dart test
