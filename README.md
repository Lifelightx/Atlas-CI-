# Atlas CI — Microservices CI/CD Platform

---

# 1. Overview

Atlas CI is a fully distributed microservices-based Continuous Integration (CI) platform similar to GitHub Actions, GitLab CI, and Jenkins. This project is designed as a learning-focused, production-style system to help understand how real-world CI/CD systems are architected and built.

This platform allows users to:

* Register and authenticate
* Create projects connected to Git repositories
* Define pipelines
* Trigger builds
* Execute builds inside isolated Docker containers
* View build logs
* Track build status

The system is built using a microservice architecture with independent services communicating via REST and asynchronous queues.

---

# 2. Goals of This Project

## Primary Goal

Learn how to design and build a real-world microservices system from scratch.

## Learning Objectives

By completing this project, you will learn:

* Microservice architecture design
* Service-to-service communication
* API Gateway pattern
* Asynchronous job processing
* Worker-based architecture
* Queue systems (Redis / BullMQ)
* Docker container orchestration
* Distributed logging
* Authentication using JWT
* Database design for distributed systems
* Infrastructure orchestration using Docker Compose

---

# 3. Business Requirements Document (BRD)

## 3.1 System Name

Atlas CI Platform

---

## 3.2 System Purpose

Atlas CI provides a distributed CI platform allowing developers to automate build, test, and container creation workflows.

---

## 3.3 Target Users

* Software developers
* DevOps engineers
* Backend engineers

---

## 3.4 Functional Requirements

### Authentication

Users must be able to:

* Register
* Login
* Receive JWT tokens
* Access protected endpoints

---

### Project Management

Users must be able to:

* Create projects
* Connect Git repositories
* View project list

---

### Pipeline Management

Users must be able to:

* Create pipelines
* Define pipeline steps
* Trigger pipeline manually

Example steps:

* Install dependencies
* Run tests
* Build Docker image

---

### Build Execution

System must:

* Execute builds asynchronously
* Run builds in isolated Docker containers
* Track build status

Statuses:

* pending
* running
* success
* failed

---

### Logging

System must:

* Capture build logs
* Store logs
* Allow users to view logs

---

### Notifications

System must:

* Notify system when build completes
* Notify when build fails

---

## 3.5 Non-Functional Requirements

### Scalability

System must support multiple workers processing builds concurrently.

### Reliability

System must recover from worker failures.

### Isolation

Each build must run in isolated container.

### Maintainability

Each service must be independently deployable.

---

# 4. System Architecture

Atlas CI follows microservice architecture.

Services:

* API Gateway
* Auth Service
* Project Service
* Pipeline Service
* Worker Service
* Log Service
* Notification Service
* Redis Queue
* PostgreSQL Database

---

# 5. Architecture Diagram (Logical)

```
User
  │
  ▼
API Gateway
  │
  ├── Auth Service
  ├── Project Service
  ├── Pipeline Service
  │        │
  │        ▼
  │     Redis Queue
  │        │
  │        ▼
  │     Worker Service
  │        │
  │        ▼
  │     Log Service
  │
  └── Notification Service

Database: PostgreSQL
Queue: Redis
Execution: Docker
```

---

# 6. Microservices Description

---

## 6.1 API Gateway

Responsibilities:

* Entry point for all requests
* Routes requests to appropriate service
* Verifies authentication

Endpoints example:

```
/api/auth
/api/projects
/api/pipelines
/api/builds
/api/logs
```

---

## 6.2 Auth Service

Responsibilities:

* Register users
* Login users
* Generate JWT tokens

Database Table:

users

Fields:

* id
* email
* password_hash
* created_at

---

## 6.3 Project Service

Responsibilities:

* Create project
* Store Git repository URL
* List projects

Table:

projects

Fields:

* id
* name
* git_url
* user_id

---

## 6.4 Pipeline Service

Responsibilities:

* Create pipeline
* Trigger builds
* Store pipeline steps

Tables:

pipelines
builds

---

## 6.5 Worker Service

Responsibilities:

* Receive build job from queue
* Run Docker container
* Execute commands
* Send logs
* Update build status

This is the core CI engine.

---

## 6.6 Log Service

Responsibilities:

* Store logs
* Provide logs

Table:

logs

---

## 6.7 Notification Service

Responsibilities:

* Receive build completion event
* Handle notifications

---

## 6.8 Redis Queue

Responsibilities:

* Store build jobs
* Distribute jobs to workers

---

## 6.9 PostgreSQL Database

Stores:

* users
* projects
* pipelines
* builds
* logs

---

# 7. Technology Stack

Backend:

* Node.js
* Express or NestJS

Database:

* PostgreSQL

Queue:

* Redis
* BullMQ

Container:

* Docker

Infrastructure:

* Docker Compose

---

# 8. Project Folder Structure

```
atlas-ci/

api-gateway/
auth-service/
project-service/
pipeline-service/
worker-service/
log-service/
notification-service/

shared/

infrastructure/
  docker-compose.yml

README.md
```

---

# 9. Communication Pattern

## Synchronous (REST)

Used for:

* Gateway → Auth
* Gateway → Project
* Gateway → Pipeline

## Asynchronous (Queue)

Used for:

* Pipeline → Worker
* Worker → Log Service

---

# 10. Build Execution Flow

```
User triggers pipeline

API Gateway

Pipeline Service

Redis Queue

Worker Service

Docker Container executes steps

Worker sends logs

Log Service stores logs

Pipeline Service updates status
```

---

# 11. Development Plan

Phase 1: Infrastructure

* Setup Docker Compose
* Setup Postgres
* Setup Redis

Phase 2: Auth Service

* Register
* Login

Phase 3: Project Service

* Create project
* List projects

Phase 4: Pipeline Service

* Create pipeline
* Trigger pipeline

Phase 5: Worker Service

* Execute jobs

Phase 6: Log Service

* Store logs

Phase 7: API Gateway

* Route requests

Phase 8: Notification Service

---

# 12. Docker Compose Services

Services:

* api-gateway
* auth-service
* project-service
* pipeline-service
* worker-service
* log-service
* notification-service
* postgres
* redis

---

# 13. Example Pipeline Definition

Example JSON:

```
{
  "steps": [
    "npm install",
    "npm test",
    "docker build -t myapp ."
  ]
}
```

---

# 14. Minimum Viable Product (MVP)

Required services:

* Auth Service
* Project Service
* Pipeline Service
* Worker Service
* Redis
* Postgres

---

# 15. Advanced Features (Future)

* GitHub webhook integration
* Live log streaming
* Kubernetes deployment
* Multiple workers
* Horizontal scaling

---

# 16. Rules While Building

Must follow:

* Each service must have its own folder
* Each service must run independently
* Services communicate via API or queue
* Do not combine services

---

# 17. Expected Outcome

After completing this project, you will understand:

* How GitHub Actions works internally
* How Jenkins works internally
* How microservices communicate
* How distributed systems work

This project represents real production architecture.

---

# 18. Final Notes

Write all code yourself.

Do not copy full implementations.

Focus on understanding:

* Why services exist
* Why queues exist
* Why workers exist

This will transform your backend engineering skills.

---

End of Document
