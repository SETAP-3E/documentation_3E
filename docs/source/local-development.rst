Local Development
=================

This section explains how to run the frontend and backend locally during development.

Flutter Frontend
----------------

Use this workflow when you want to work on the Flutter web UI without running the full Docker stack.

1. Open a terminal and navigate to the frontend folder:

.. code-block:: console

   cd frontend

2. Install dependencies:

.. code-block:: console

   flutter pub get

3. Run the app in Chrome:

.. code-block:: console

   flutter run -d chrome

To run on a specific port:

.. code-block:: console

   flutter run -d chrome --web-port 3000

The app will open at `http://localhost:3000` or the selected port.

Hot Reload / Hot Restart
~~~~~~~~~~~~~~~~~~~~~~~~

While `flutter run` is active, use the following keyboard shortcuts in the terminal:

- ``r`` — hot reload
- ``R`` — hot restart
- ``q`` — quit

Dart Frog Backend
-----------------

Use this workflow when you want to work on the API locally.

1. Install the Dart Frog CLI if it is not already installed:

.. code-block:: console

   dart pub global activate dart_frog_cli

2. Navigate to the backend folder and install dependencies:

.. code-block:: console

   cd backend
   dart pub get

3. Start the Dart Frog development server:

.. code-block:: console

   dart_frog dev

The API will be available at `http://localhost:8080`.

Notes
-----

- When running the frontend locally, update the frontend API base URL if needed so it points to the local backend.
- If you use the Docker setup instead, the backend will already be available at `http://localhost:8080`.
