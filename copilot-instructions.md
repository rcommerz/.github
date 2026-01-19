# RCOMMERZ Microservices Platform - Copilot Instructions

## Project Overview
Enterprise-grade rcommerz platform built with microservices architecture, event-driven design, and cloud-native technologies.


- [x] Verify that the copilot-instructions.md file in the .github directory is created.

- [ ] Clarify Project Requirements
	**Status:** ✅ Complete
	- Project: E-commerce microservices platform
	- 12 backend microservices (Laravel, Go, Python, Node.js)
	- 2 frontend applications (Next.js 14)
	- Infrastructure: Kafka, Redis, MySQL, MongoDB, PostgreSQL, Elasticsearch, ClickHouse
	- Gateway: Kong with Keycloak OAuth2/OIDC
	- Deployment: Docker Compose (local) + Kubernetes/Minikube

- [ ] Scaffold the Project
	**In Progress:** Creating microservices project structure
	- Backend services with Dockerfiles
	- Frontend applications
	- Infrastructure configurations
	- Kubernetes manifests
	- Development tooling

- [ ] Customize the Project
	- Implement service code for all microservices
	- Configure inter-service communication
	- Set up event-driven patterns
	- Configure API Gateway routes

- [ ] Install Required Extensions
	- N/A - Extensions handled by user workspace

- [ ] Compile the Project
	- Build Docker images for all services
	- Verify dependencies
	- Run health checks

- [ ] Create and Run Task
	- Docker Compose up/down tasks
	- Minikube deployment tasks
	- Individual service development tasks

- [ ] Launch the Project
	- Start infrastructure (databases, Kafka, Redis)
	- Deploy services to Docker Compose
	- Access via Kong Gateway

- [ ] Ensure Documentation is Complete
	- README with setup instructions
	- Service-specific documentation
	- API documentation (OpenAPI/Swagger)
	- Deployment guides

## Development Guidelines
- Use Docker for local development
- Kubernetes manifests ready for Minikube
- Event-driven architecture with Kafka
- Zero-trust security with Keycloak + Kong
- LGTM observability stack

## ⚠️ Reserved Host Ports (CRITICAL - Avoid Conflicts)

### Infrastructure Services (In Use)
**Database Ports:**
- `3306` - MySQL
- `5432` - PostgreSQL (shared)
- `5433` - TimescaleDB
- `6379` - Redis
- `27017` - MongoDB
- `9200, 9300` - Elasticsearch
- `8123, 9004` - ClickHouse
- `9002, 9003` - MinIO (S3)

**Message Broker:**
- `9092, 19092, 9093` - Kafka
- `8090` - Kafka UI

**API Gateway & Auth:**
- `8080, 8443, 8001` - Kong Gateway
- `8085` - Keycloak
- `1337` - Konga (Kong Admin)

**Observability (LGTM Stack):**
- `3100` - Loki (Logs)
- `3200, 9411` - Tempo (Traces)
- `9009` - Mimir (Metrics)
- `3002` - Grafana (Dashboards)
- `4317, 4318, 8888, 8889, 13133, 55679` - OpenTelemetry Collector
- `12345` - Alloy (Telemetry Collector)

**Stream Processing:**
- `8081` - Flink JobManager

### Application Services (Reserved)
**Frontend Applications:**
- `13000` - Storefront (Customer Next.js Frontend) - changed from 3000
- `13001` - Admin Panel (Next.js Frontend and backend) - changed from 3001

**Backend Microservices:**
- `13010` - User Service (Laravel) - changed from 3010
- `13020` - Product Service (Node.js) - changed from 3020
- `13030` - Cart Service (Go) - changed from 3030
- `13040` - Order Service (Go) - changed from 3040
- `13050` - Payment Service (Go) - changed from 3050
- `13060` - Inventory Service (Go) - changed from 3060
- `13070` - Shipping Service (Go) - changed from 3070
- `13080` - Review Service (Node.js) - changed from 3080
- `13090` - Notification Service (Go) - changed from 3090
- `13110` - Analytics Service (Python) - changed from 3110
- `13120` - Reporting Service (Python) - changed from 3120
- `13130` - Recommendation Service (Python) - changed from 3130

### Port Allocation Rules for New Services
1. **NEVER expose host ports already listed above**
2. **Container ports can be standard** (e.g., 80, 8080, 3000) but map to available host ports
3. **Use port range 13140+ for new microservices**
4. **Internal services don't need host port mapping** (use Docker network only)
5. **Document any new host port in PORT_MAPPING.md**


### Example Docker Compose Port Mapping
```yaml
# ✅ CORRECT - Avoids conflicts using 13000+ range
services:
  new-service:
    ports:
      - "13140:3000"  # Host:Container

# ❌ WRONG - Conflicts with infrastructure or existing services
services:
  new-service:
    ports:
      - "3000:3000"  # Port 3000 conflicts!
      - "8080:8080"  # Port 8080 conflicts!

# ✅ BEST - No host port for internal services
services:
  internal-service:
    # No ports section - accessible only within rcommerz-network
    networks:
      - rcommerz-network
```

### Port Reference Documentation
See `platform-gitops/clusters/minikube/docker-compose/PORT_MAPPING.md` for complete port mapping details.


INSTRUCTIONS GENERATE DOCKER FILE AND DOCKER COMPOSER:

Generate a **single multi-stage Dockerfile** and a **single docker-compose.yml** for an application.

REQUIREMENTS:

1. Dockerfile:

   - Use **one Dockerfile** with **multi-stage builds**.
   - Include the following stages:
     a. base – install dependencies and set up working directory.
     b. local – development stage with hot reload / live coding if applicable.
     c. builder – build the application for production.
     d. prod – minimal production image with only runtime dependencies.
   - Expose port 8080 (default).
   - Use a non-root user in the production stage.
   - Avoid duplicating logic between stages.

2. docker-compose.yml:

   - Use a **single compose file** for both local development and production.
   - Build stage selectable via environment variable `BUILD_TARGET`:
       - BUILD_TARGET=local → local development stage.
       - BUILD_TARGET=prod → production stage.
   - Image tag configurable via environment variable `TAG`, defaulting to `local`.
   - Mount source code volume only for local builds.
   - Pass an environment variable `APP_ENV` to the container.

3. Constraints:

   - Output **only the Dockerfile and docker-compose.yml**.
   - Do **not** generate explanations, extra commands, CI/CD, or Kubernetes configs.
   - The prompt should work for **any language or runtime** (language-agnostic).

USAGE:

- For local development: `docker compose up --build`
- For production build: `BUILD_TARGET=prod TAG=1.0.0 docker compose build`
