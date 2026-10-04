# Book Library System

A modular Book Library System demonstrating REST APIs, PostgreSQL, Docker, Nginx, automated testing, and Jenkins CI/CD.

## Three modules

1. **Book Management** — CRUD for books.
2. **Library Search** — searches OpenLibrary through an external API.
3. **User & Loan Management** — manages users and borrowing/returning books.

## Architecture

Browser -> Nginx -> Express API -> PostgreSQL

The Library Search module also communicates with OpenLibrary.

## Run with Docker

```bash
docker compose up -d --build
```

Open `http://localhost:8080`.

API health check: `http://localhost:8080/health`

## Run locally

Install PostgreSQL, create a database named `library`, then set the DB environment variables if your credentials differ from the defaults.

```bash
npm install
npm start
```

## Run tests

```bash
npm test
```

## Jenkins

The `Jenkinsfile` performs checkout, dependency installation, database startup, automated tests, Docker image build, deployment, and a smoke test.
