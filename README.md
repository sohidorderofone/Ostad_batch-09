# Django Deployment on Render

This document describes the steps I followed to deploy a Django application on **Render**.

---

## 1. Preparing Django Project Locally

### A. Add production-safe host and CSRF handling

In `settings.py`, add:

```python
CSRF_TRUSTED_ORIGINS = [
    "https://ostad-batch-09.onrender.com",
    "http://localhost:8000",
    "http://127.0.0.1:8000",
]
```
# Add the following configuration in settings.py:

```
STATIC_URL = "/static/"
STATIC_ROOT = BASE_DIR / "staticfiles"
```

This is what fixed the collectstatic error during deployment.
# C. Ensure gunicorn is included in dependencies
Example requirements.txt:

```
Django>=5.2,<6.0
gunicorn
```
Gunicorn is the production WSGI server used by Render.

# 2. Push Code to GitHub
Render deploys directly from GitHub, so push your changes:
```
git add .
git commit -m "Prepare Django app for Render deployment"
git push origin main
```
# 3. Create the Render Web Service

Steps:

1. Go to Render Dashboard
2. Click New → Web Service
3. Connect your GitHub repository
4. Select branch: main
5. Choose environment: Python
   
Render automatically provisions a Python runtime environment.

# 4. Configure Build and Start Commands
# Build Command
``` pip install -r requirements.txt && python manage.py migrate && python manage.py collectstatic --noinput ```

What this command does:

* Installs dependencies
* Applies database migrations
* Collects static files into STATIC_ROOT

# Start Command
``` gunicorn todo_project.wsgi:application --bind 0.0.0.0:$PORT ```


What this command does:

* Starts the Django application using Gunicorn
* Binds the app to the port provided by Render ($PORT)

# 5. Deploy and Fix Common Errors
``` Error: STATIC_ROOT not set ```

Fix by ensuring this exists in settings.py:

``` STATIC_ROOT = BASE_DIR / "staticfiles" ```

Fix by adding your Render domain (or .onrender.com) to ALLOWED_HOSTS.

# 6. Confirm Deployment

Visit the deployed application:

``` https://ostad-batch-09.onrender.com/ ```

You should see this screen for time being, just wait <img width="1346" height="606" alt="image" src="https://github.com/user-attachments/assets/4e162018-f6a5-4cc2-bb2e-c3104dcffed8" />

You will navigate to app page after a while<img width="880" height="584" alt="image" src="https://github.com/user-attachments/assets/81f02e35-b770-4110-8be2-30172b7bd3b6" />

