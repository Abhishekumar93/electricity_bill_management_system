# Electricity Bill Management System — Backend

> **Project Status Notice**
> This is an older learning and portfolio project. It is no longer actively maintained or deployed and may require its original development environment and dependency versions to run. The repository is preserved to demonstrate the concepts explored at the time rather than current production practices.

---

## Overview

This repository contains the Django REST backend developed as part of an Electricity Bill Management System learning project.

The backend focuses on user management, authentication workflows, and role handling for consumers and staff members. It primarily implements:

- Consumer and staff accounts
- Account registration
- Account activation through email
- Token-based login
- OTP-based login
- Consumer and staff listing
- User filtering and pagination
- User detail retrieval
- User account updates
- Email templates

_Note: This repository primarily implements user account and authentication services; it does not calculate electricity bills, generate invoices, or process payments._

---

## Frontend Relationship

This backend was built to serve the companion frontend application:

- **Frontend Repository:** [https://github.com/Abhishekumar93/ebms-frontend](https://github.com/Abhishekumar93/ebms-frontend)

The frontend interface was designed to consume this backend's REST APIs for user registration, authentication, and directory views. Because the backend service is currently offline, authentication and data-dependent actions in the public frontend preview will not complete.

---

## Historical Technology Stack

The project relies on the following historical pinned dependencies (as listed in `requirements.txt`):

- **Python:** Use a version compatible with the pinned dependencies
- **Django:** 4.2.3
- **Django REST Framework:** 3.14.0
- **Database Driver:** `psycopg2-binary` 2.9.6 (PostgreSQL)
- **Filtering:** `django-filter` 23.2
- **CORS Headers:** `django-cors-headers` 4.2.0
- **Authentication Support:** `dj-rest-auth` 4.0.1
- **Token Authentication:** Django REST Framework Token Authentication (`rest_framework.authtoken`)
- **Environment Configuration:** `python-dotenv` 1.0.0

---

## Project Structure

A high-level view of the key directories and files in this repository:

- `electricity_bill_management_system/` — Django project configuration, settings, root URL routing, and custom token authentication.
- `portal_user/` — Django application containing user models, serializers, views, URL routes, custom token generators, and email templates.
- `portal_user/migrations/` — Database migrations for the custom user model and fields.
- `templates/` — Shared Django base templates.
- `manage.py` — Django administrative command-line entry point.
- `requirements.txt` — Historical pinned Python package dependencies.

---

## Implemented API Routes

The following endpoints are defined across the project URL configuration:

| HTTP Method     | Route                                                   | Purpose                                                                                 | Authentication Expected                       |
| :-------------- | :------------------------------------------------------ | :-------------------------------------------------------------------------------------- | :-------------------------------------------- |
| `POST`          | `/api-token-auth/`                                      | Authenticate user via password or OTP (validating consumer/staff role) and return token | No (`AllowAny`)                               |
| `POST`          | `/portal-user/api/create/user/`                         | Register a new user account and send activation & welcome emails                        | No (`AllowAny`)                               |
| `GET`           | `/portal-user/api/login/`                               | Retrieve basic user data for the current authenticated user                             | Yes (`IsAuthenticated`)                       |
| `POST`          | `/portal-user/api/otp/`                                 | Generate a temporary OTP and send it via email                                          | No (`AllowAny`)                               |
| `GET`           | `/portal-user/api/list/user/`                           | List all users with pagination and filtering (`is_active`, `user_role`)                 | Yes (`IsAuthenticated`, `IsAdminUser`)        |
| `PUT` / `PATCH` | `/portal-user/api/user/update/<int:pk>/`                | Update user details by user ID                                                          | Yes (`IsAuthenticated`, `IsAdminUser`)        |
| `GET`           | `/portal-user/api/user/detail/<int:pk>/`                | Retrieve detailed profile data for a specific user ID                                   | Yes / Read-Only (`IsAuthenticatedOrReadOnly`) |
| `GET`           | `/portal-user/api/activate/<slug:uidb64>/<slug:token>/` | Activate a user account from an email link                                              | No (`AllowAny`)                               |
| `POST`          | `/dj-rest-auth/logout/`                                 | Invalidate current session/token                                                        | No / Session                                  |

---

## Historical Local Setup

> **Warning:** These setup instructions reflect the historical project configuration and have not been revalidated against current Python, PostgreSQL, or operating system releases.

### 1. Clone the repository

```bash
git clone https://github.com/Abhishekumar93/electricity_bill_management_system.git
cd electricity_bill_management_system
```

### 2. Create and activate a virtual environment

```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Set up PostgreSQL

Create a PostgreSQL database and dedicated user:

```sql
CREATE DATABASE <database_name>;
CREATE USER <database_user> WITH PASSWORD '<database_password>';
ALTER ROLE <database_user> SET client_encoding TO 'utf8';
ALTER ROLE <database_user> SET default_transaction_isolation TO 'read committed';
ALTER ROLE <database_user> SET timezone TO 'UTC';
GRANT ALL PRIVILEGES ON DATABASE <database_name> TO <database_user>;
```

### 5. Configure environment variables

Set the required environment variables (see [Environment Variables](#environment-variables) below) in your local environment or `.env` file.

### 6. Run database migrations

```bash
python manage.py migrate
```

### 7. Create a superuser (optional)

```bash
python manage.py createsuperuser
```

### 8. Start the development server

```bash
python manage.py runserver
```

---

## Environment Variables

The Django settings reference the following environment variable names. Values should be populated based on your target environment:

### Core Application

- `SECRET_KEY` — Django secret key for cryptographic signing.
- `DEBUG` — Boolean string (`"True"` or `"False"`) to enable/disable debug mode and toggle database/email configurations.
- `CLIENT_DOMAIN` — Frontend domain URL used for constructing email activation links.

### Local Database (`DEBUG="True"`)

- `DB_NAME` — PostgreSQL database name.
- `DB_USER` — PostgreSQL database user.
- `DB_PASSWORD` — PostgreSQL database password.
- `DB_HOST` — PostgreSQL host address (e.g., `localhost`).
- `DB_PORT` — PostgreSQL port (e.g., `5432`).

### Remote / Production Database (`DEBUG="False"`)

- `RDS_DB_NAME` — Remote PostgreSQL database name.
- `RDS_USERNAME` — Remote PostgreSQL database user.
- `RDS_PASSWORD` — Remote PostgreSQL database password.
- `RDS_HOSTNAME` — Remote PostgreSQL host address.
- `RDS_PORT` — Remote PostgreSQL port.

### Email Configuration (SMTP)

- `EMAIL_HOST` — SMTP server address (when `DEBUG="False"`).
- `EMAIL_PORT` — SMTP server port.
- `EMAIL_HOST_USER` — SMTP user email address (also used as default sender).
- `EMAIL_HOST_PASSWORD` — SMTP user password / app password.

### Cloud / Storage Configuration

- `AWS_ACCESS_KEY_ID` — AWS access key identifier.
- `AWS_SECRET_ACCESS_KEY` — AWS secret access key.

---

## Known Limitations

- **Not Currently Deployed:** The backend is offline and not actively hosted.
- **Maintenance Status:** The project is an older learning project and is not actively maintained.
- **Compatibility:** The API and dependencies have not been revalidated against current dependency versions or modern Python releases.
- **Scope:** The project is primarily a demonstration of user authentication, custom user models, and role management rather than a comprehensive billing or payment processing system.
- **External Services:** Account activation and OTP features rely on a properly configured SMTP email provider.
- **Frontend Dependency:** The public companion frontend cannot perform live authentication or data fetching while this backend remains offline.
- **Design Context:** The codebase reflects learning-stage implementation decisions from the time it was authored.

---

## Learning Outcomes

This project was built to explore and demonstrate several full-stack and backend concepts:

- Implementing a custom user model in Django (`AbstractBaseUser`, `PermissionsMixin`)
- Building RESTful endpoints with Django REST Framework (DRF)
- Integrating PostgreSQL with Django using `psycopg2`
- Managing token-based authentication (`rest_framework.authtoken` and `dj-rest-auth`)
- Handling user email activation via cryptographic tokens (`PasswordResetTokenGenerator`, base64 encoding)
- Implementing custom OTP generation and email dispatch for passwordless login
- Defining distinct role-based access for staff and consumer accounts
- Query filtering using `django-filter` and custom pagination
- Rendering dynamic HTML email templates with Django template engine
- Connecting and configuring CORS for decoupled frontend/backend communication
