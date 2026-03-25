# Local Implementation Guide

Step-by-step instructions to build and run the application on your local machine using three methods: Maven, Docker, or Docker Compose.

---

## Prerequisites

| Tool | Minimum Version | Check |
|---|---|---|
| Java (JDK) | 17 | `java -version` |
| Maven | 3.8 | `mvn -version` |
| Docker | 24 | `docker --version` |

> Docker is only required for the Docker / Docker Compose methods.

---

## Method 1 — Maven (Fastest)

```bash
# 1. Clone the repository
git clone https://github.com/<your-username>/Spring-Boot-Sample-Project.git
cd Spring-Boot-Sample-Project

# 2. Run the app
mvn spring-boot:run
```

App starts at: **http://localhost:9090**

---

## Method 2 — Build JAR and Run

```bash
# 1. Package the JAR (skips tests for speed)
mvn clean package -DskipTests

# 2. Run the JAR
java -jar target/hello-service-1.0.0.jar
```

App starts at: **http://localhost:9090**

---

## Method 3 — Docker

```bash
# 1. Build the image
docker build -t hello-service:latest .

# 2. Run the container
docker run -d \
  --name hello-service \
  -p 9090:9090 \
  hello-service:latest
```

App starts at: **http://localhost:9090**

Useful Docker commands:
```bash
docker logs -f hello-service     # stream logs
docker stop hello-service        # stop container
docker rm hello-service          # remove container
```

---

## Method 4 — Docker Compose (Recommended for local dev)

```bash
docker compose up
```

To run in background:
```bash
docker compose up -d
docker compose logs -f           # stream logs
docker compose down              # stop and remove
```

App starts at: **http://localhost:8080**

> Note: Docker Compose maps to port `8080` on your host. The other methods use `9090` (the app's native port).

---

## Run Tests

```bash
mvn test
```

Test reports are generated at: `target/surefire-reports/`

---

## Verify the App is Running

Once started, hit these endpoints:

```bash
# Root endpoint
curl http://localhost:9090/
# {"message":"Hello from MyApp CI/CD Pipeline!","status":"UP","timestamp":"..."}

# Greeting endpoint
curl http://localhost:9090/hello/John
# {"greeting":"Hello, John! 👋"}

# Health check
curl http://localhost:9090/actuator/health
# {"status":"UP",...}

# App info
curl http://localhost:9090/actuator/info
# {"app":{"name":"hello-service","version":"1.0.0",...}}
```

---

## Troubleshooting

**Port already in use**
```bash
# Find what's using port 9090
lsof -i :9090          # macOS / Linux
netstat -ano | findstr :9090   # Windows
```

**Maven not found**
```bash
# macOS
brew install maven

# Ubuntu / Debian
sudo apt install maven
```

**Docker build fails on `mvn` not found**
The Dockerfile uses a multi-stage build — Maven runs inside the builder container, so it does not need to be installed on your host for Docker builds.

**Java version mismatch**
```bash
java -version   # must show 17 or higher
```
If you have multiple JDKs, set `JAVA_HOME` to point to JDK 17.
