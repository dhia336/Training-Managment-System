# Training Center Management System

A web application for managing a training center's participants, trainers, and training cycles. The project combines a React frontend with a FastAPI backend and a MySQL database, with an authenticated dashboard for managing records and viewing summary statistics.

## Features

- **Admin authentication:** HTTP Basic credentials for login and JWT bearer tokens for authenticated API requests.
- **Dashboard statistics:** counts for participants, trainers, and training cycles, plus breakdowns by company, topic, and trainer specialty.
- **Participant management:** list, search, filter, paginate, inspect, create, update, and delete participant records through the API.
- **Trainer management:** view and maintain trainer profiles, specialties, departments, and organizations.
- **Training cycle management:** manage cycle identifiers, topics, dates, rooms, and assigned trainers.
- **Data export:** API export endpoint for downloading database data.
- **Interactive frontend:** React, React Router, Axios, GSAP animations, and internationalization support.
- **Database helpers:** SQL views, stored procedures, indexes, and a sample database dump.

## Tech stack

| Layer | Technologies |
| --- | --- |
| Frontend | React, Vite, React Router, Axios |
| UI and interaction | CSS, GSAP, React Icons |
| Internationalization | i18next, react-i18next |
| Backend | Python, FastAPI, Uvicorn |
| Authentication | bcrypt password hashing, JWT bearer tokens |
| Database | MySQL / MariaDB, mysql-connector-python |

## Repository structure

```text
Training-Managment-System/
├── app/
│   ├── main.py              # FastAPI application and router registration
│   ├── config.py            # Database and JWT configuration
│   ├── database.py          # MySQL connection helper
│   ├── security.py          # Password verification and JWT utilities
│   └── routes/              # Authentication, participant, trainer, and cycle APIs
├── frontend/
│   ├── src/                 # React pages, API client, and UI components
│   └── package.json         # Frontend dependencies and scripts
├── database.sql             # SQL schema, views, procedures, and sample data
└── main.py                  # Backend entry point
```

## Requirements

- Python 3.10 or newer recommended
- Node.js and npm
- MySQL 8+ or a compatible MariaDB version
- Git

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/dhia336/Training-Managment-System.git
cd Training-Managment-System
```

### 2. Create and configure the database

Create a database in MySQL or MariaDB, then import `database.sql`. For example, using the MySQL command-line client:

```sql
CREATE DATABASE bf_arabe1 CHARACTER SET utf8mb4;
```

Then import the dump:

```bash
mysql -u YOUR_DB_USER -p bf_arabe1 < database.sql
```

The dump includes sample records and defines database objects using a local `root@localhost` definer. If your database user differs, review the dump's `DEFINER` clauses and adjust them as needed before importing. Use a development database because the dump contains sample data.

### 3. Configure the backend

Create a `.env` file in the repository root. The backend loads configuration from environment variables:

```dotenv
DB_HOST=localhost
DB_USER=your_mysql_user
DB_PASSWORD=your_mysql_password
DB_NAME=bf_arabe1
SECRET_KEY=replace-with-a-long-random-secret
```

Install the backend dependencies:

```bash
python -m venv .venv
```

Activate the virtual environment.

**Linux / macOS**
```bash
source .venv/bin/activate
```

**Windows PowerShell**
```powershell
.venv\Scripts\Activate.ps1
```

Install the packages used by the backend:

```bash
python -m pip install fastapi uvicorn mysql-connector-python python-dotenv bcrypt python-jose
```

Start the API from the repository root:

```bash
uvicorn app.main:app --reload --host 127.0.0.1 --port 8000
```

The API root is available at `http://127.0.0.1:8000/`; interactive API documentation is available at `http://127.0.0.1:8000/docs`.

### 4. Configure and start the frontend

Open a second terminal:

```bash
cd frontend
npm install
```

The frontend API client defaults to `http://localhost:8000/api`. If your backend uses a different address, create `frontend/.env` and set:

```dotenv
VITE_API_BASE_URL=http://localhost:8000/api
```

Start the development server:

```bash
npm run dev
```

Vite prints the local frontend URL in the terminal, commonly `http://localhost:5173`.

## API overview

The backend registers its routers under the `/api` prefix. The main endpoints include:

| Method | Endpoint | Purpose |
| --- | --- | --- |
| `POST` | `/api/login` | Authenticate an administrator using HTTP Basic credentials and return a JWT |
| `GET` | `/api/stats` | Return dashboard statistics |
| `GET` | `/api/participants` | List participants; supports theme, company, name, limit, and offset filters |
| `GET` | `/api/participants/{id}` | Retrieve one participant |
| `GET` | `/api/formateurs` | List trainers |
| `GET` | `/api/cycles` | List training cycles |

Most management routes require an `Authorization: Bearer <token>` header. Consult the interactive API documentation at `/docs` for request parameters and the remaining create, update, and delete endpoints.

## Security and project status

This is a portfolio / learning project, not a production-ready deployment.

- Set a unique, strong `SECRET_KEY` in the environment. The source configuration contains an insecure fallback value that should never be used in production.
- Do not expose the sample database or use its sample administrator accounts on a public server. Create new credentials and replace all demo data.
- Review CORS origins before deployment; the current backend configuration includes development addresses.
- Review token storage, session expiry, input validation, error handling, database permissions, and HTTPS configuration before using real participant data.
- The repository does not currently include a dedicated backend requirements file or documented automated test suite.

## License

No license file was confirmed during this documentation update. Check repository licensing before reusing or redistributing the code.
