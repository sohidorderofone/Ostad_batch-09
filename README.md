1) Preparing Django project locally
A) Add production-safe host + debug handling

In settings.py:


CSRF_TRUSTED_ORIGINS = [
    "https://ostad-batch-09.onrender.com", 
    "http://localhost:8000",
    "http://127.0.0.1:8000",
]

B) Fix static files for production (this was required)
STATIC_URL = "/static/"
STATIC_ROOT = BASE_DIR / "staticfiles"


This is what fixed the collectstatic error.

C) Making sure gunicorn in requirements.txt

Example requirements.txt:

Django>=5.2,<6.0
gunicorn

2) Push code to GitHub - Render will deploy from GitHub

3) Create the Render Web Service

Render Dashboard → New → Web Service

Connect to GitHub repo

Pick branch main 

Environment: Python

4) Set Render Build + Start commands
Build Command
pip install -r requirements.txt && python manage.py migrate && python manage.py collectstatic --noinput


What it does:

installs dependencies

applies DB migrations

collects static files into STATIC_ROOT

Start Command
gunicorn todo_project.wsgi:application --bind 0.0.0.0:$PORT


What it does:

starts the app using Gunicorn

binds to Render’s provided $PORT

5) Deploy and fix common errors
A) If you see: STATIC_ROOT not set


6) Confirm deployment

Visit: https://ostad-batch-09.onrender.com/

Check admin, pages, CSS loads

Check Render logs for errors