# nfp-backend

Setup:

    python -m venv venv
    . venv/bin/activate
    pip install -r requirements.txt

Run the development server:

    uvicorn main:app --reload

Url:
https://www.travisluong.com/how-to-build-a-full-stack-next-js-fastapi-postgresql-boilerplate-tutorial/

Initial Project Setup

- Let’s create a directory to put all of our code. In a real project, you may want to separate your front-end and back-end repositories, but for the convenience of this tutorial, we’ll throw everything into one repo.

$ mkdir nfp-boilerplate
$ cd nfp-boilerplate

- Next, let’s get the back end running. First, start by creating a directory for the FastAPI application in the project root.

$ mkdir nfp-backend
$ cd nfp-backend

- Create and activate the python virtual environment.

(MacBook)
$ python -m venv venv
$ . venv/bin/activate

(Windows)
cd venv\Scripts
activate

To deactivate the virtual environment and return to the global Python environment, simply type:

deactivate

- Install FastAPI, and other dependencies.

$ pip install fastapi "uvicorn[standard]" gunicorn psycopg2 sqlalchemy alembic "databases[postgresql]" python-dotenv

- Here is a quick description of each package.

fastapi – web framework
uvicorn – asgi server
gunicorn – wsgi server
psycopg2 – postgresql driver
sqlalchemy – python sql toolkit and object relational mapper
databases – asyncio support for databases
alembic – database migration tool

- Freeze the requirements.

$ pip freeze > requirements.txt

- Database Setup – PostgreSQL
  Create the dev database.

$ createdb nfp_boilerplate_dev

Create a user. The -P flag will issue a prompt for the password of the new user.

$ createuser nfp_boilerplate_user -P

- Migration Tool Setup – Alembic
  In the nfp-backend directory, initialize alembic.

$ alembic init alembic

In alembic.ini, find the line with this text sqlalchemy.url = driver://user:pass@localhost/dbname. Replace it with:

sqlalchemy.url = postgresql://nfp_boilerplate_user:password@localhost/nfp_boilerplate_dev
Generate the first migration file.

$ alembic revision -m "create notes table"

Preview the SQL that will be run by the migration.

$ alembic upgrade head --sql

To actually run the migration, run the same command without the --sql flag:

$ alembic upgrade head

Log into the DB using psql.

$ psql nfp_boilerplate_dev
Run \dt command to show the tables. You should see the newly created notes table along with the alembic_version table.

To revert one migration, run:

$ alembic downgrade -1
To run one migration, run:

$ alembic upgrade +1
You can change the number to run or revert multiple migrations.

- Run the backend server.

$ uvicorn main:app --reload

- Go to http://127.0.0.1:8000/docs. You should see the auto-generated API documentation. This documentation is interactive, which means you can make requests to your API from this interface. Go ahead and try to post a note and get a note.

Verify that it has been saved in the database.

$ psql nfp_boilerplate_dev

# select \* from notes;
