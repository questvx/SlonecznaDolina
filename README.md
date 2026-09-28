# Sloneczna Dolina

A desktop application for the day-to-day operations of a ski resort. It brings staff workflows for sales, equipment rentals, ski school services, instructors, and resort scheduling into one place.

This repository contains the **complete solution**: a desktop application and its REST API backend, not just a frontend.

## Features

- Records sales of lift passes, ski school lessons, and rental equipment.
- Supports user accounts and JWT-based authentication.
- Organizes instructors, events, and work schedules.
- Stores operational data in a MySQL relational database, with schema migrations managed by Flyway.
- Runs the staff interface as an Electron desktop application.

## Architecture and tech stack

| Area | Technologies |
| --- | --- |
| Desktop application | Electron 30, React 18, TypeScript, Vite, React Router |
| REST API | Java 21, Spring Boot 3.5, Spring Web, Spring Security, JWT |
| Data and migrations | Spring Data JPA, MySQL, Flyway |
| Testing and API documentation | Maven, JUnit/Spring Boot Test, Springdoc OpenAPI |

The frontend communicates with the backend over HTTP. In local development, it expects the API at `http://localhost:8080`.

## Repository structure

```text
.
├── SlonecznaDolina/         # Electron + React desktop application
│   ├── electron/            # Electron main process and preload script
│   └── src/                 # UI and domain views
└── slonecznadolinaBackend/  # Spring Boot API, database integration, migrations
    └── src/main/resources/db/migration/
```

## Prerequisites

- Node.js 18 or later and npm
- JDK 21
- A local or otherwise accessible MySQL instance

## Run locally

### 1. Configure the database and start the backend

Create a MySQL database and configure the database connection and JWT secret in the backend configuration (`slonecznadolinaBackend/src/main/resources/application.yaml` or through Spring environment variables). Do not commit real passwords or secrets.

From the repository root, start the backend:

```powershell
cd slonecznadolinaBackend
./mvnw.cmd spring-boot:run
```

The backend serves the API on port `8080` by default. Interactive OpenAPI documentation is available at `http://localhost:8080/swagger-ui/index.html`.

### 2. Start the desktop application

In a second terminal, from the repository root:

```powershell
cd SlonecznaDolina
npm install
npm run dev
```

Development mode starts Vite alongside the Electron process. Keep the backend and MySQL running when using features that require the API.

## Build and checks

Backend commands, run from `slonecznadolinaBackend`:

```powershell
./mvnw.cmd test
./mvnw.cmd package
```

Frontend and desktop application commands, run from `SlonecznaDolina`:

```powershell
npm run lint
npm run build
```

The desktop build uses electron-builder. The configuration defines a Windows installer, a macOS DMG, and a Linux AppImage.

## Security

Provide JWT secrets and database credentials locally through environment variables or private development configuration. Do not commit `.env` files containing real secrets.
