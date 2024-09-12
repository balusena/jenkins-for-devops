# Jenkins CI-CD Pipeline for ML-App (Money Authentication)

The **ML_APP_Docker_Jenkins_Automation** project integrates continuous integration and continuous deployment (CI/CD) using
**Linux, GitHub, Jenkins, and Docker** for a machine learning-based money authentication application. This pipeline automates
the process from code commit to deployment, ensuring efficient software delivery and maintaining consistency across environments.

## Tools Overview

- **Linux**: Provides the underlying infrastructure for Jenkins and Docker, ensuring a reliable and stable environment.
- **GitHub**: Serves as the version control system, allowing developers to collaboratively work on the ML application’s codebase. The pipeline is triggered automatically via GitHub webhooks when code is pushed.
- **Jenkins**: Acts as the orchestrator for CI/CD, pulling the latest code from GitHub, building the application inside a Docker container, and running tests. Jenkins ensures that every commit is verified with automated builds and tests.
- **Docker**: Is used to package the ML application into a container. This ensures that the application runs consistently across different environments, from testing to production.

Once the Docker image is built, it is pushed to DockerHub, ready for deployment. This automated pipeline improves efficiency,
minimizes human error, and accelerates the release cycle of the ML-based money authentication app.
