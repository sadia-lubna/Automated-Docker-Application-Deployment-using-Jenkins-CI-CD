# Automated Docker Application Deployment using Jenkins CI/CD

A DevOps project demonstrating how to **containerize a multi-service application and automate application deployment using Docker and Jenkins CI/CD**.

The project uses the open-source **Craftista** application and focuses on the DevOps side of the implementation, including Docker containerization, service networking, Docker Compose, and CI/CD automation.

---

## Project Overview

The application consists of four services:

* **Frontend** — Node.js application
* **Catalogue** — Python Flask application
* **Recommendation** — Go application
* **Voting** — Java Spring Boot application

Each service runs in its own Docker container and communicates with other services through a custom Docker bridge network.

The deployment process is being developed progressively:

```text
Docker
   ↓
Docker Compose
   ↓
Jenkins CI/CD
   ↓
Kubernetes
   ↓
Prometheus & Grafana
   ↓
Argo CD / GitOps
```

---

## Architecture

```text
                    ┌─────────────────────┐
                    │      Frontend       │
                    │      Node.js        │
                    │      Port 3000      │
                    └──────────┬──────────┘
                               │
              ┌────────────────┼────────────────┐
              │                │                │
              ▼                ▼                ▼
     ┌────────────────┐ ┌──────────────┐ ┌──────────────┐
     │   Catalogue    │ │ Recommendation│ │    Voting    │
     │    Flask       │ │      Go       │ │ Java/Spring  │
     │    :5000       │ │     :8080     │ │    :8080     │
     └────────────────┘ └──────────────┘ └──────────────┘

                    Docker Network
                  craftista-network
```

---

## Technologies Used

### Application

* Node.js
* Python / Flask
* Go
* Java / Spring Boot
* H2 Database

### DevOps

* Linux
* Git
* GitHub
* Docker
* Docker Compose
* Jenkins
* CI/CD

### Planned

* Kubernetes
* Prometheus
* Grafana
* Argo CD
* GitOps

---

## Repository Structure

```text
Automated-Docker-Application-Deployment-using-Jenkins-CI-CD/
│
├── Dockerfiles/
│   ├── catalogue.Dockerfile
│   ├── frontend.Dockerfile
│   ├── recommendation.Dockerfile
│   └── voting.Dockerfile
│
├── docker-compose.yml
└── README.md
```

The application source code is maintained separately in the Craftista repository.

---

## Dockerfiles

Separate Dockerfiles are maintained for each application service.

### Frontend

```dockerfile
FROM node:21-alpine
```

Runs the Node.js frontend on port `3000`.

### Catalogue

```dockerfile
FROM python:3.11-slim
```

Runs the Flask catalogue service on port `5000`.

### Recommendation

Uses a multi-stage Docker build with Go:

```dockerfile
FROM golang:1.20-alpine AS builder
```

The application runs on port `8080`.

### Voting

Uses Maven and Eclipse Temurin Java 17:

```dockerfile
FROM maven:3.9-eclipse-temurin-17 AS builder
```

The application runs on port `8080`.

---

## Docker Compose

Docker Compose is used to build and run all four services together.

```bash
docker compose up -d
```

Check running containers:

```bash
docker compose ps
```

View logs:

```bash
docker compose logs
```

View logs for a specific service:

```bash
docker compose logs frontend
docker compose logs catalogue
docker compose logs recco
docker compose logs voting
```

Stop the application:

```bash
docker compose down
```

Rebuild images:

```bash
docker compose build
```

Rebuild and start:

```bash
docker compose up -d --build
```

---

## Service Ports

| Service        | Container Port | Host Port |
| -------------- | -------------: | --------: |
| Frontend       |           3000 |      3000 |
| Catalogue      |           5000 |      5000 |
| Recommendation |           8080 |      8081 |
| Voting         |           8080 |      8082 |

The application can be accessed through:

```text
http://localhost:3000
```

---

## Docker Networking

All services communicate through a custom Docker bridge network:

```text
craftista-network
```

Service-to-service communication uses Docker DN

