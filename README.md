# A RESTful API todo list creator/tool

Using a RESTful API, create, find, update and delete tasks, as well as register new users.

## Installation

clone this repo, create a virtual environment, and install the requirements. Mac/Linux:

    $ virtualenv venv
    $ source venv/bin/activate
    (venv) $ pip install -r requirements.txt

For Windows users:

    $ virtualenv venv
    $ venv\Scripts\activate
    (venv) $ pip install -r requirements.txt

After installation is complete, create a '.env' file with the following:

    export SECRET_KEY = 'your_secret_key_here'
    export SQLALCHEMY_DATABASE_URI = 'sqlite:///db.sqlite'

## Running the API Server

To run the server, simply run:

    (venv) $ python todoapi.py
     * Running on http://127.0.0.1:5000/
     * Restarting with reloader

Open another terminal window and you can begin making requests!

## API Documentation

  - POST /api/users

    Register a new user.
    The body must contain a JSON object that defines username and password fields.
    On success, a status code 201 is returned. The body of the response contains a JSON object with the newly added user. A Location header contains the URI of the new user.
    On failure, status code 400 (bad request) is returned.
    Notes:
        - The password is hashed before it is stored in the database. Once hashed, the original password is discarded.
        + In a production deployment secure HTTP must be used to protect the password in transit.

  - GET /api/users/<int:id>
    
    Return a user.
    On success, a status code 200 is returned. The body of the response contains a JSON object with the requested user.
    On failure, status code 400 (bad request) is returned.

  - GET /api/token
    Return an authentication token.
    This request must be authenticated using a HTTP Basic Authentication header.
    On success, a JSON object is returned with a field token set to the authentication token for the user and a field duration set to the (approximate) number of seconds the token is valid.
    On failure, status code 401 (unauthorized) is returned.

  - GET /api/resource

    Return a protected resource.
    This request must be authenticated using a HTTP Basic Authentication header. Instead of username and password, the client can provide a valid authentication token in the username field. If using an authentication token the password field is not used and can be set to any value.
    On success, a JSON object with data for the authenticated user is returned.
    On failure, status code 401 (unauthorized) is returned.

  - GET /todo/api/v1.0/tasks

    Return a json object of all tasks for the user.
    This request must be authenticated using a HTTP Basic Authentication header. Instead of username and password, the client can provide a valid authentication token in the username field. If using an authentication token the password field is not used and can be set to any value.
    On success, a JSON object with data for the authenticated user is returned.
    On failure, status code 401 (unauthorized) is returned.

  - POST /todo/api/v1.0/tasks

    Create a new task for the user.
    This request must be authenticated using a HTTP Basic Authentication header. Instead of username and password, the client can provide a valid authentication token in the username field. If using an authentication token the password field is not used and can be set to any value.
    On success, a new task is created and a JSON object with data for the authenticated user is returned.
    On failure, status code 401 (unauthorized) is returned.

  - GET /todo/api/v1.0/tasks/<int:task_id>

    Return a json object of the requested task for the user.
    This request must be authenticated using a HTTP Basic Authentication header. Instead of username and password, the client can provide a valid authentication token in the username field. If using an authentication token the password field is not used and can be set to any value.
    On success, a JSON object with data for the authenticated user is returned.
    On failure, a status code 401 (unauthorized) is returned.

  * PUT /todo/api/v1.0/tasks/<int:task_id>

    Update an existing task for the user.
    The incoming request must in unicode format.
    If the information is not unicode, return status code 400 (bad request) is returned.
    This request must be authenticated using a HTTP Basic Authentication header. Instead of username and password, the client can provide a valid authentication token in the username field. If using an authentication token the password field is not used and can be set to any value.
    On success, a JSON object with data for the authenticated user's updated task is returned.


  - DELETE /todo/api/v1.0/tasks/<int:task_id>

    Delete an existing task for the user, if that task exists.
    This request must be authenticated using a HTTP Basic Authentication header. Instead of username and password, the client can provide a valid authentication token in the username field. If using an authentication token the password field is not used and can be set to any value.
    On success, a JSON object containing a 'True' result is returned.
    On failure,  a status code 404 (not found) is returned.


## Future Improvements

  - Next implementations would include:
    
    Adding a web interface to allow users to bypass the API and browse their todo list via their browser.

    Create an email functionality to send a formatted email with all tasks or specific tasks to the user requested email address (web interface)

    Create a downloadable CSV file for requested tasks (all, some, or one) for the web interface