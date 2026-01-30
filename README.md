# First Aid Kit Manager - Deployment Hub

![Docker](https://img.shields.io/badge/Docker-Required-blue)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-17.4-blue)
![Java](https://img.shields.io/badge/Java-25-orange)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-4.0.1-green)
![React](https://img.shields.io/badge/React-19-61dafb)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)

**Stop throwing money in the trash.** The average household loses hundreds of dollars every year on expired medications that pile up unnoticed in cabinets and drawers. First Aid Kit Manager puts you back in control — it tracks every drug in your home pharmacy, warns you before anything expires, and makes sure you never waste another pill or penny. Set it up once, and let the app do the rest.

One-command deployment for the complete First Aid Kit Manager stack. This repository contains Docker Compose configuration to run the entire application (Frontend + Backend API + PostgreSQL database) with a single command.

## Key Features

- **Multi-tenancy with JWT** - Secure user authentication with isolated data per user
- **Automatic Email Alerts** - Notifications for medications nearing expiration
- **Advanced Filtering** - Search drugs by name, form, expiration date with pagination
- **Modern Tech Stack** - Java 25, Spring Boot 4.0.1, React 19, PostgreSQL 17.4
- **One-Command Deployment** - Get the full stack running in minutes

## Quick Start

### 1. Prerequisites

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed and running
- Terminal (bash, zsh, PowerShell)

### 2. Environment Setup

Copy the example environment file and fill in your credentials:

```bash
cp .env.example .env
```

Edit `.env` and configure:

```dotenv
# PostgreSQL
POSTGRES_USER=your_db_user
POSTGRES_PASSWORD=your_secure_password

# Email (Gmail with App Password)
MAIL_USERNAME=your_email@gmail.com
MAIL_PASSWORD=your_app_password

# JWT Secret (min 32 characters)
JWT_SECRET=YourSecureJWTSecretKeyAtLeast32Characters
```

### 3. Launch

```bash
docker compose up -d
```

That's it! Wait for containers to start (first run may take a few minutes to pull images).

### Access Points

| Service | URL | Description |
|---------|-----|-------------|
| Frontend UI | http://localhost | Main web application |
| Backend API | http://localhost:8080 | REST API |
| pgAdmin | http://localhost:5050 | Database management (optional) |

## System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     Docker Network                          │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   ┌─────────────┐     ┌─────────────┐     ┌─────────────┐  │
│   │  Frontend   │────▶│   Backend   │────▶│  PostgreSQL │  │
│   │   (React)   │     │ (Spring Boot)│     │    (DB)     │  │
│   │   :80       │     │   :8080     │     │   :5432     │  │
│   └─────────────┘     └─────────────┘     └─────────────┘  │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Service Details

| Service | Image | Port | Description |
|---------|-------|------|-------------|
| frontend | `ghcr.io/nextstepprodev/first-aid-kit-web:master` | 80 | React 19 web application |
| api | `ghcr.io/nextstepprodev/first-aid-kit-api:master` | 8080 | Spring Boot REST API |
| postgres | `postgres:17.4` | 5432 | PostgreSQL database |

## Common Commands

```bash
# Start all services
docker compose up -d

# Stop all services
docker compose down

# View logs
docker compose logs -f

# View logs for specific service
docker compose logs -f api

# Restart a service
docker compose restart api

# Rebuild and restart
docker compose up -d --build
```

## Troubleshooting

### Port 8080 Already in Use

On macOS, AirPlay Receiver uses port 5000 and sometimes conflicts occur on 8080.

**Solution:**
1. Go to System Preferences → General → AirDrop & Handoff
2. Disable "AirPlay Receiver"

Or check what's using the port:
```bash
lsof -i :8080
```

### CORS Policy Errors

If you see CORS errors in the browser console, ensure you're accessing the frontend via `http://localhost` (port 80), not `http://localhost:3000`.

### View Service Logs

```bash
# All services
docker compose logs -f

# Backend only
docker compose logs -f api

# Database only
docker compose logs -f postgres
```

### Database Reset

To completely reset the database (removes all data):

```bash
docker compose down -v
docker compose up -d
```

## Environment Variables Reference

| Variable | Required | Description |
|----------|----------|-------------|
| `POSTGRES_USER` | Yes | Database username |
| `POSTGRES_PASSWORD` | Yes | Database password |
| `MAIL_USERNAME` | Yes | Gmail address for sending alerts |
| `MAIL_PASSWORD` | Yes | Gmail App Password |
| `JWT_SECRET` | Yes | Secret key for JWT tokens (min 32 chars) |
| `PGADMIN_DEFAULT_EMAIL` | No | pgAdmin login email |
| `PGADMIN_DEFAULT_PASSWORD` | No | pgAdmin login password |

## Security Best Practices

- **Never commit `.env`** - It's already in `.gitignore`
- **Use strong passwords** - Especially for database and JWT secret
- **Gmail App Passwords** - Use [App Passwords](https://support.google.com/accounts/answer/185833), not your regular Gmail password
- **Change defaults** - Don't use example values in production

## Related Repositories

| Repository | Description |
|------------|-------------|
| [first-aid-kit-api](https://github.com/NextStepProDev/first-aid-kit-api) | Backend REST API (Spring Boot) |
| [first-aid-kit-frontend](https://github.com/NextStepProDev/first_aid_kit_frontend) | Frontend application (React) |

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

Made with care by [Mateusz Nawratek](https://www.linkedin.com/in/mateusz-nawratek-909752356)
