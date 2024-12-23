# Docker CI/CD Pipeline for EC2 Deployment

This repository implements a CI/CD pipeline that builds Docker images for both a web and an API service, pushes them to Docker Hub, and deploys the updated services to an AWS EC2 instance. The pipeline uses GitHub Actions and Docker Compose to automate the process.

## Overview

This setup consists of the following services:

- **Web**: A frontend service that communicates with the API.
- **API**: A backend service that communicates with a PostgreSQL database.
- **Database**: A PostgreSQL container for storing data.
- **Nginx**: A reverse proxy server to route traffic between the web and API services.


---

## Workflow

The GitHub Actions workflow defined in `.github/workflows/main.yml` consists of the following jobs:

### Job 1: Build and Push Web Docker Image:
- **Description**: Builds the Docker image for the web service located in the `./web` directory.
- **Action**: Pushes the built image to Docker Hub.

### Job 2: Build and Push API Docker Image:
- **Description**: Builds the Docker image for the API service located in the `./api` directory.
- **Action**: Pushes the built image to Docker Hub.

###Job 3: Deploy to EC2 Server:
- **Description**: Once the web and API Docker images are built and pushed:
  - SSHs into the EC2 instance.
  - Install Docker: If Docker is not installed
  - Install Docker Compose: If Docker Compose is not installed
  - Stop and Remove  existing Containers
  - Remove Existing Directory
  - Restart Docker Compose Services:


---

## Getting Started

### 1. Clone the repository:
   Clone this repository to your local machine or EC2 instance.

### 2. Define Environment Variables:
   - Create a `.env` file in the root directory to define environment-specific values (e.g., API and DB configuration).

### 3. Configure GitHub Secrets:
   - Set up GitHub secrets for sensitive data like Docker credentials, EC2 access details, and personal access tokens.

### 4. Setup Docker Compose:
   Docker Compose is used to manage the multi-tier architecture, including the web service, API service, database, and Nginx reverse proxy.

---

## Configuration

### Environment Variables

Define the following variables in your `.env` file:

- **API Service Variables**:
  - Port for the API service.
  - Database connection string.

- **Web Service Variables**:
  - Port for the Web service.
  - API endpoint.

- **Database Service Variables**:
  - PostgreSQL user, password, and database name.

### GitHub Secrets

In your GitHub repository settings, add the following secrets to secure your credentials:

- `DOCKER_USERNAME`: Your Docker Hub username.
- `DOCKER_PASSWORD`: Your Docker Hub password.
- `EC2_HOST`: The public IP address of your EC2 instance.
- `EC2_USERNAME`: The SSH username for your EC2 instance.
- `EC2_PRIVATE_KEY`: The private key file used to access the EC2 instance.
- `TOKEN`: GitHub Personal Access Token for cloning the repository.

---

## Nginx Configuration

Nginx serves as a reverse proxy to route traffic between the web and API services. It ensures that users are directed to the correct service based on the request.

---

## Accessing the Application

Once deployed, you can access the application by visiting the public IP address of your EC2 instance:

- **Web Service**: `http://<EC2_PUBLIC_IP>:3000`
- **API Service**: `http://<EC2_PUBLIC_IP>:4001`

---

## Troubleshooting

- Ensure Docker and Docker Compose are properly installed and configured on the EC2 instance.
- Double-check GitHub secrets to ensure correct credentials.
- Review logs in case of build or deployment failures.


---

## Inbound Rules Configuration
Ensure to configure them **EC2 Security Groups** for setting up the rules.

---
