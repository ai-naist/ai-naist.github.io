---
layout: post
title: "Setting Up a Rye + Django + Angular Environment"
categories: [development, tutorial]
tags: [rye, django, angular, python, javascript, setup]
---

This guide summarizes the steps for setting up a development environment using Rye, Django, and Angular.

## 1. Introduction

A summary of the procedure for setting up an environment with Rye + Django + Angular.

## 2. Prerequisites

This guide assumes that the following tools are already installed:

*   **Rye:** Python package and virtual environment management tool.
*   **Node.js & npm:** JavaScript runtime and package manager.
*   **Angular CLI:** Angular project management tool (`npm install -g @angular/cli`).
*   **MySQL Server:** Database server.
*   **Git:** Version control system.

## 3. Project Setup

### 3.1. Create Root Directory and Initialize Git

```bash
# Create the project root directory and navigate into it
mkdir scheduling-app
cd scheduling-app

# Initialize a Git repository
git init
```

### 3.2. Backend Setup (Django + Rye)

```bash
# Create the backend directory and navigate into it
mkdir backend
cd backend

# Initialize the Rye project (Rye detects Python version or specify with rye pin <version>)
rye init .
# rye pin 3.11 # Specify version if needed

# Add required Python packages
rye add django djangorestframework mysqlclient

# Sync dependencies (Rye might do this automatically)
rye sync

# Create the Django project (in the current directory)
rye run django-admin startproject backend_project .

# Create a Django application for scheduling
rye run python manage.py startapp scheduling

# --- Edit backend/backend_project/settings.py ---
# - Add 'rest_framework' and 'scheduling' to INSTALLED_APPS
# - Change DATABASES setting for MySQL configuration
# - Set TIME_ZONE (e.g., 'UTC' or 'Asia/Tokyo'), LANGUAGE_CODE (e.g., 'en-us' or 'ja'), etc.
# --------------------------------------------------

# Run the initial database migrations
rye run python manage.py migrate

# Return to the root directory
cd ..
```

### 3.3. Frontend Setup (Angular)

```bash
# Create the Angular project in the frontend directory (with routing, using SCSS)
# (Run in the scheduling-app directory)
ng new frontend --routing --style=scss

# Navigate into the frontend directory
cd frontend

# Add Angular Material
ng add @angular/material

# --- Angular Material Setup Prompts ---
# ? Choose a prebuilt theme name, or "custom" for a custom theme:
#   (Use arrow keys to select and press Enter. Example: Indigo/Pink)
# ? Set up global Angular Material typography styles? (Y/n) Y
# ? Set up browser animations for Angular Material? (Y/n) Y
# ----------------------------------------------------

# Return to the root directory
cd ..
```

## 4. Running Development Servers and Connection

During development, you'll run the backend and frontend servers simultaneously. Requests from the frontend to the backend API will be forwarded via a proxy.

### 4.1. Start the Backend Server

```bash
# Open a new terminal or run in the current one
cd backend
rye run python manage.py runserver
# (Starts by default at http://localhost:8000)
cd .. # Return to the root directory if needed
```

### 4.2. Start the Frontend Server (with Proxy Configuration)

**Initial Error:** When trying to start the frontend server with the proxy configuration, an error occurred because `proxy.conf.json` did not exist.

```bash
# (Example error when run in the scheduling-app/frontend directory)
# ng serve --proxy-config proxy.conf.json
# > An unhandled exception occurred: Proxy configuration file C:\...\scheduling-app\frontend\proxy.conf.json does not exist.
```

**Solution:**

1.  **Create `proxy.conf.json`:** Create a `proxy.conf.json` file directly inside the `frontend` directory.
2.  **Add content to the file:** Add the following content to `proxy.conf.json` and save it.

    ```json
    {
        "/api": {
            "target": "http://localhost:8000",
            "secure": false,
            "changeOrigin": true
        }
    }
    ```

3.  **Run the start command again:**

    ```bash
    # Make sure you are in the frontend directory
    cd frontend

    # Start the Angular development server with proxy configuration enabled
    ng serve --proxy-config proxy.conf.json
    # (Starts by default at http://localhost:4200)
    ```

Now, requests from the frontend application starting with `/api/**` will be forwarded to the backend at `http://localhost:8000/api/**`.

## 5. Final Directory Structure (Key Parts)

```plaintext
scheduling-app/
├── .git/
├── .gitignore
├── backend/
│   ├── .venv/
│   ├── .python-version
│   ├── pyproject.toml
│   ├── backend_project/
│   │   ├── __init__.py
│   │   ├── settings.py
│   │   ├── urls.py
│   │   ├── wsgi.py
│   │   └── asgi.py
│   ├── scheduling/
│   │   ├── __init__.py
│   │   ├── admin.py
│   │   ├── apps.py
│   │   ├── migrations/
│   │   ├── models.py
│   │   ├── serializers.py # (To be created)
│   │   ├── tests.py
│   │   ├── urls.py      # (To be created)
│   │   └── views.py     # (To be created)
│   ├── manage.py
│   └── requirements.lock
├── frontend/
│   ├── .angular/
│   ├── node_modules/
│   ├── src/
│   │   ├── app/
│   │   ├── assets/
│   │   ├── environments/
│   │   ├── index.html
│   │   ├── main.ts
│   │   └── styles.scss
│   ├── .editorconfig
│   ├── .gitignore
│   ├── angular.json
│   ├── package.json
│   ├── package-lock.json
│   ├── proxy.conf.json   # <- Created file
│   ├── tsconfig.app.json
│   ├── tsconfig.json
│   └── tsconfig.spec.json
└── README.md
```
