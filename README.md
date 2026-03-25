# Spring Boot Sample Project — End-to-End DevOps Pipeline

[![CI/CD](https://github.com/<your-username>/Spring-Boot-Sample-Project/actions/workflows/ci.yml/badge.svg)](https://github.com/<your-username>/Spring-Boot-Sample-Project/actions)
[![License: Apache 2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](LICENSE)
[![Java](https://img.shields.io/badge/Java-17-orange.svg)](https://openjdk.org/projects/jdk/17/)
[![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.2.4-brightgreen.svg)](https://spring.io/projects/spring-boot)
[![Docker](https://img.shields.io/badge/Docker-multi--stage-blue.svg)](Dockerfile)

A **production-ready Spring Boot 3 REST service** demonstrating a complete DevOps pipeline — from source code to a containerized deployment on AWS ECR, with automated code quality, security scanning, and CI/CD via Jenkins and GitHub Actions.

---

## Table of Contents

- [Architecture Overview](#architecture-overview)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [CI/CD Pipeline](#cicd-pipeline)
- [Getting Started](#getting-started)
- [API Endpoints](#api-endpoints)
- [Security & Code Quality](#security--code-quality)
- [Docker](#docker)
- [Contributing](#contributing)

---

## Architecture Overview

```
Developer Push
      │
      ▼
┌─────────────┐     ┌──────────────┐     ┌─────────────────┐
│   GitHub    │────▶│   Jenkins /  │────▶│  SonarQube      │
│  (Source)   │     │  GH Actions  │     │  (Code Quality) │
└─────────────┘     └──────┬───────┘     └─────────────────┘
                           │
              ┌────────────┼────────────┐
              ▼            ▼            ▼
       ┌────────────┐ ┌─────────┐ ┌──────────┐
       │   Maven    │ │  OWASP  │ │  Trivy   │
       │ Unit Tests │ │  Dep    │ │  Image   │
       │            │ │  Check  │ │  Scan    │
       └────────────┘ └─────────┘ └──────────┘
                           │
                           ▼
                  ┌─────────────────┐
                  │   Docker Build  │
                  │  (Multi-stage)  │
                  └────────┬────────┘
                           │
                           ▼
                  ┌─────────────────┐
                  │    AWS ECR      │
                  │  (Image Store)  │
                  └────────┬────────┘
                           │
                           ▼
                  ┌─────────────────┐
                  │   Deployment    │
                  │  (Docker Host)  │
                  └─────────────────┘
```

---

## Tech Stack

| Layer | Tool / Technology |
|---|---|
| Language | Java 17 |
| Framework | Spring Boot 3.2.4 |
| Build | Maven 3.x |
| Containerization | Docker (multi-stage, non-root) |
| CI/CD | Jenkins Pipeline + GitHub Actions |
| Code Quality | SonarQube |
| Security Scan | OWASP Dependency-Check, Trivy |
| Image Registry | AWS ECR |
| Health Monitoring | Spring Boot Actuator |

---

## Project Structure

```
Spring-Boot-Sample-Project/
├── src/
│   ├── main/java/com/myapp/hello/
│   │   ├── Application.java          # Spring Boot entry point
│   │   └── HelloController.java      # REST controller
│   ├── main/resources/
│   │   └── application.properties    # App configuration
│   └── test/java/com/myapp/hello/
│       └── HelloControllerTest.java  # Unit tests
├── .github/
│   ├── workflows/
│   │   └── ci.yml                    # GitHub Actions CI/CD
│   ├── ISSUE_TEMPLATE/
│   │   ├── bug_report.md
│   │   └── feature_request.md
│   └── pull_request_template.md
├── Dockerfile                        # Multi-stage Docker build
├── docker-compose.yml                # Local development setup
├── Jenkinsfile                       # Jenkins declarative pipeline
├── pom.xml                           # Maven build config
├── CONTRIBUTING.md
└── SECURITY.md
```

---

## CI/CD Pipeline

### Jenkins Pipeline Stages

```
Checkout → Compile → Unit Test → SonarQube Scan → Quality Gate
    → OWASP Dependency Check → Trivy FS Scan → Build JAR
    → Docker Build → Trivy Image Scan → Push to ECR → Deploy
```

### GitHub Actions

Triggers on every push and pull request to `main`:
- Build & test with Maven
- SonarCloud analysis
- Docker build verification

---

## Getting Started

### Prerequisites

- Java 17+
- Maven 3.8+
- Docker 24+

### Run Locally

```bash
# Clone the repo
git clone https://github.com/<your-username>/Spring-Boot-Sample-Project.git
cd Spring-Boot-Sample-Project

# Build and run with Maven
mvn spring-boot:run

# Or with Docker Compose
docker compose up
```

App starts on `http://localhost:8080`

### Run Tests

```bash
mvn test
```

### Build Docker Image

```bash
docker build -t hello-service:latest .
docker run -p 8080:8080 hello-service:latest
```

---

## API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| GET | `/hello` | Returns a greeting message |
| GET | `/actuator/health` | Application health status |
| GET | `/actuator/info` | Application metadata |

### Example

```bash
curl http://localhost:8080/hello
# {"message":"Hello, World!"}

curl http://localhost:8080/actuator/health
# {"status":"UP"}
```

---

## Security & Code Quality

- **SonarQube** — static code analysis, code smells, coverage gates
- **OWASP Dependency-Check** — scans Maven dependencies for known CVEs
- **Trivy** — scans filesystem and Docker image for vulnerabilities (blocks on CRITICAL/HIGH)
- **Non-root Docker user** — container runs as `appuser` (UID 1001)
- **Multi-stage Docker build** — minimal runtime image, no build tools in production

---

## Docker

The `Dockerfile` uses a two-stage build:

1. **Builder stage** — Amazon Corretto 17, compiles and packages the JAR
2. **Runtime stage** — headless Amazon Corretto 17, runs as non-root user

```bash
# Build
docker build -t hello-service:1.0.0 .

# Run
docker run -d -p 8080:8080 --name hello-service hello-service:1.0.0
```

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on how to contribute to this project.

---

## License

This project is licensed under the [Apache License 2.0](LICENSE).
