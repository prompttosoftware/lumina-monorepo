# Lumina - An AI-Powered Personalized Learning Platform

## Overview

Lumina is an innovative, AI-powered learning platform designed to revolutionize education through personalized experiences. It dynamically adapts to individual student needs, provides real-time feedback, and offers intuitive dashboards for educators and administrators to monitor progress and manage learning content efficiently. Our goal is to create an engaging and effective learning environment that caters to the unique journey of every student.

### Key Features:

*   **Personalized Learning Paths**: AI-driven recommendations adapting to student performance and learning style.
*   **Real-time Feedback**: Instantaneous feedback on quizzes and exercises to guide students.
*   **Comprehensive Dashboards**: Tailored views for students, teachers, and administrators to track progress, manage content, and gain insights.
*   **Content Management**: Tools for teachers to create, organize, and publish diverse learning materials (lessons, quizzes, assignments).
*   **Assessment Engine**: Robust system for administering and evaluating student assessments, with automated grading capabilities.

### Target Users:

*   **Students**: To engage with dynamic learning content, receive personalized guidance, and track their educational growth.
*   **Teachers**: To effortlessly create courses, organize content, monitor class performance, and interact with student data.
*   **Administrators**: To oversee platform operations, manage users, and analyze high-level usage and performance metrics.

## Getting Started

This repository contains the monorepo for the Lumina platform, encompassing all microservices and frontend applications.

### Prerequisites

Before you begin, ensure you have the following installed:

*   **Docker Desktop**: For containerization of all services.
*   **Kubernetes (Minikube/Kind for local development, or a cloud-managed cluster)**: Our deployment target.
*   **Python 3.9+**: For backend service development.
*   **Node.js 18+ & npm/yarn**: For frontend development.
*   **git**: For version control.

### Installation & Local Setup

1.  **Clone the Repository:**
    ```bash
    git clone https://github.com/your-org/lumina-monorepo.git
    cd lumina-monorepo
    ```

2.  **Environment Variables:**
    Create a `.env` file at the root of the project (or within respective service directories as needed) based on `.env.example` templates. This will include database credentials, JWT secrets, AWS configuration, etc.
    ```
    # Example .env content for root
    DATABASE_URL="postgresql://user:password@localhost:5432/lumina_db"
    JWT_SECRET_KEY="your-super-secret-jwt-key"
    AWS_ACCESS_KEY_ID="your_aws_access_key"
    AWS_SECRET_ACCESS_KEY="your_aws_secret_key"
    AWS_REGION="us-east-1"
    # ... other variables per service
    ```

3.  **Database Setup (Local):**
    For local development, you can use Docker Compose to spin up a PostgreSQL instance.
    ```bash
    # From the monorepo root
    docker-compose -f docker-compose.db.yml up -d
    ```
    Ensure your `DATABASE_URL` in `.env` points to this local instance.

4.  **Install Dependencies:**
    Each service will have its own `requirements.txt` (Python) or `package.json` (Node.js). You'll typically install these within their respective directories.

    *   **Backend Services (e.g., `services/auth/`)**:
        ```bash
        cd services/auth
        pip install -r requirements.txt
        ```
    *   **Frontend Application (`frontend/`)**:
        ```bash
        cd frontend
        npm install # or yarn install
        ```

5.  **Run Services Locally:**
    Each service can be run independently for development.

    *   **Backend Services (e.g., `services/auth/`)**:
        ```bash
        cd services/auth
        uvicorn app.main:app --reload --port 8000 # or the specific port for the service
        ```
    *   **Frontend Application (`frontend/`)**:
        ```bash
        cd frontend
        npm start # or yarn start
        ```

Access the frontend application at `http://localhost:3000` (or as configured). The backend APIs will be available on their respective ports (e.g., `http://localhost:8000/auth`).

## Architecture

Lumina adopts a microservices architecture, organized within a single monorepository. This structure balances independent service development and deployment with simplified initial management.

### Technology Stack:

*   **Backend**: Python (FastAPI)
*   **Frontend**: React.js with TypeScript
*   **Databases**: PostgreSQL (primary), MongoDB (for specific use cases later on)
*   **AI/ML**: PyTorch / TensorFlow
*   **Caching**: Redis
*   **Message Broker**: RabbitMQ / Kafka (future consideration; initial lightweight queuing)
*   **Containerization**: Docker
*   **Orchestration**: Kubernetes
*   **Cloud Provider**: AWS (EC2, RDS, EKS, S3, Lambda, SQS/SNS, CloudFront)
*   **CI/CD**: GitHub Actions / GitLab CI/CD

### Conceptual Diagram:

```
[User Devices]
      |
      | (HTTP/S)
      V
[Load Balancer (AWS ALB)]
      |
      +---------------------------------+-------------------------------------+
      |                                 |                                     |
      V                                 V                                     V
[Frontend Service (React App)]    [API Gateway Service (FastAPI)]    [Auth Service (FastAPI)]
      |                                 |                                     |
      +---------------------------------+-------------------------------------+
      |                                 |                                     |
      V                                 V                                     V
[User Service]                    [Course Service]                     [Content Service]
(FastAPI)                           (FastAPI)                            (FastAPI)
      |                                 |                                     |
      V                                 V                                     V
[Assessment Service]              [Feedback Service]                   [Adaptive Learning Service]
(FastAPI)                           (FastAPI)                            (FastAPI/ML)
      |                                 |                                     |
      +---------------------------------+-------------------------------------+
      |                                 |
      V                                 V
[PostgreSQL DB (AWS RDS)]         [Redis Cache (ElastiCache)]       [S3 Storage (Static Assets)]
      ^                                 ^                                       ^
      |                                 |                                       |
      +---------------------------------+---------------------------------------+
      |                                 |
      V                                 V
[Message Broker (SQS/SNS)] <--- [Analytics Service (FastAPI)]
(Async Events/Reporting)
```

## Core Services

The platform is composed of several independent microservices:

*   **Auth Service**: Handles user registration, login, and JWT token management.
*   **User Service**: Manages user profiles (students, teachers, administrators) and role-based access.
*   **Course Service**: Manages the creation, organization, and enrollment in courses.
*   **Content Service**: Stores and serves various learning content types (lessons, quizzes, assignments), integrates with S3 for media.
*   **Assessment Service**: Administers quizzes and assignments, collects responses, and performs automated grading.
*   **Feedback Service**: Provides real-time, rule-based feedback on student answers during exercises.
*   **Adaptive Learning Engine (ALE) Service**: Analyzes student performance to generate personalized learning paths and content recommendations.
*   **Analytics Service**: Aggregates data for reporting and dashboards across different user roles.
*   **Frontend Service**: The React.js application serving the user interface for all roles.

## Development

### Monorepo Structure

```
.
├── auth/                       # Auth Service
├── user/                       # User Service
├── course/                     # Course Service
├── content/                    # Content Service
├── assessment/                 # Assessment Service
├── feedback/                   # Feedback Service
├── ale/                        # Adaptive Learning Engine Service
├── analytics/                  # Analytics Service
├── frontend/                   # React Frontend Application
├── ci-cd/                      # CI/CD configurations (e.g., GitHub Actions workflows)
├── docs/                       # Project documentation
├── scripts/                    # Utility scripts (e.g., deploy, setup)
├── .env.example                # Example environment variables
└── docker-compose.db.yml       # Local database setup
```

### Contributing

We welcome contributions! Please refer to our `CONTRIBUTING.md` (to be created) for guidelines on how to submit pull requests, report bugs, and suggest features.

### Testing

A robust testing strategy is in place to ensure code quality and system reliability:

*   **Unit Tests**: Isolated tests for individual functions and classes using `pytest` (Python) and `Jest` (Frontend).
*   **Integration Tests**: Verifying interactions between services and modules using FastAPI's `TestClient` (Python) and mocking external dependencies with `moto` for AWS services.
*   **End-to-End Tests**: Simulating full user journeys across the UI and backend using `Playwright`.

### Code Style

We adhere to standard Python (PEP 8) and TypeScript/ESLint conventions. Linters and formatters are integrated into the CI/CD pipeline.

## Deployment

Lumina is designed for cloud-native deployment on AWS using Kubernetes.

### CI/CD Pipeline

Our CI/CD pipeline, built with GitHub Actions/GitLab CI/CD, automates:

1.  **Code Linting & Static Analysis**
2.  **Unit & Integration Testing**
3.  **Docker Image Builds**
4.  **Image Push to AWS ECR**
5.  **Kubernetes Deployment (via `kubectl` or Helm)**
6.  **E2E Testing on Staging Environments**

Detailed deployment instructions and Kubernetes manifests will be provided in the `ci-cd/` directory.

## Monitoring & Maintenance

We employ comprehensive monitoring and logging solutions to ensure platform stability and performance:

*   **Centralized Logging**: Structured JSON logs collected in AWS CloudWatch Logs.
*   **Application Performance Monitoring (APM)**: Metrics (request rates, error rates, latency, resource usage) collected and visualized via Prometheus/Grafana or AWS CloudWatch.
*   **Distributed Tracing**: OpenTelemetry / AWS X-Ray for tracing requests across microservices.
*   **Alerting**: Configured alerts for critical issues (e.g., high error rates, service downtime).
*   **Automated Backups**: Regular database backups managed by AWS RDS.
*   **Security Audits**: Continuous vulnerability scanning and periodic penetration testing.
*   **AI Model Retraining**: Automated pipelines for updating the Adaptive Learning Engine model.

## Support & Contact

For any questions, issues, or feature requests, please open an issue in this repository.

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.
