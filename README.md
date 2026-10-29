# Jenkins CI/CD Pipeline | AWS EC2 Deployment

An automated CI/CD pipeline for a Node.js application using Jenkins, Docker, Docker Hub, GitHub, and AWS EC2.

This project combines Jenkins pipeline automation with AWS infrastructure provisioning and container deployment. It covers application testing, versioning, Docker image publishing, automated deployment to EC2, branch-based execution, and GitHub webhook integration.

## CI/CD Pipeline

The Jenkins pipeline performs the following steps:

1. Increments the Node.js application version using npm.
2. Installs dependencies and runs Jest tests.
3. Builds a versioned Docker image.
4. Pushes the image to Docker Hub.
5. Deploys the Dockerized application to AWS EC2 using SSH and Docker Compose.

### Pipeline Workflow

```text
GitHub Push
    |
    v
GitHub Webhook
    |
    v
Jenkins Multibranch Pipeline
    |
    +-- Feature Branch
    |       |
    |       v
    |    Run Tests
    |
    +-- Main Branch
            |
            v
      Increment Version
            |
            v
         Run Tests
            |
            v
      Build Docker Image
            |
            v
         Docker Hub
            |
            v
      Deploy to AWS EC2
```

## AWS Infrastructure

Provisioned and configured AWS infrastructure using the AWS CLI to host the Node.js application.

### Infrastructure Setup

- Created an IAM user and configured AWS CLI credentials and region.
- Created a dedicated VPC and subnet.
- Configured a security group for SSH and application access.
- Launched an EC2 instance with an SSH key pair.
- Connected to EC2 using SSH.
- Installed Docker to run containerized applications.
- Configured security group access to make the application accessible from a browser.

The application runs as a Docker container on an AWS EC2 instance.

## Automated Deployment with Docker Compose

Added a `docker-compose.yaml` file to define the application's container configuration.

Extended the Jenkins pipeline with a deployment stage that:

1. Connects to AWS EC2 using Jenkins-managed SSH credentials.
2. Transfers the Docker Compose configuration and deployment script to EC2.
3. Executes the deployment script remotely.
4. Deploys the application using the Docker image published to Docker Hub.

This eliminates the need to manually SSH into the server and deploy each new application version.

## Multibranch Pipeline and GitHub Webhook

Configured Jenkins to support different pipeline behavior depending on the Git branch.

- **Main branch:** Runs tests, builds and publishes the Docker image, and deploys the application to AWS EC2.
- **Feature branches:** Run tests without publishing images or deploying unfinished changes.

Added a GitHub webhook to notify Jenkins automatically when code is pushed to the repository.

This allows the pipeline to start without manually triggering a build.

## Jenkins Shared Library

Reusable pipeline functions were extracted into a separate Jenkins Shared Library.

The library includes:

- `incrementVersion()` — increments the Node.js application version.
- `runTests()` — installs dependencies and runs Jest tests.
- `buildAndPushDockerImage()` — builds and publishes versioned Docker images.
- `commitVersionUpdate()` — commits application version updates to GitHub.

The version-commit stage was used in the earlier Jenkins exercise and is currently disabled in the multibranch pipeline to prevent automated commits from repeatedly triggering webhook builds.

The shared library keeps the Jenkinsfile smaller and allows common CI/CD functionality to be reused across projects.

**Shared Library Repository:** [jenkins-shared-library](https://github.com/Tea-naa/jenkins-shared-library)

## Docker Image

The Node.js application is packaged into a Docker image and published to Docker Hub.

Example image format:

```text
teanaa/jenkins-node-app:<version>
```

The published image is used for deployment to AWS EC2.

## Tech Stack

- **Cloud:** AWS EC2, VPC, IAM, Security Groups, AWS CLI
- **CI/CD:** Jenkins, Jenkins Multibranch Pipeline, GitHub Webhooks
- **Containers:** Docker, Docker Compose, Docker Hub
- **Application:** Node.js, npm, Jest
- **Automation:** Groovy, Bash, SSH
- **Version Control:** Git, GitHub
- **Additional Infrastructure:** DigitalOcean (initial Jenkins and container hosting)

## Project Structure

```text
.
├── app/
│   ├── Dockerfile
│   ├── package.json
│   ├── package-lock.json
│   ├── server.js
│   └── server.test.js
├── Jenkinsfile
├── docker-compose.yaml
├── server-cmds.sh
└── README.md
```

## What I Practiced

- Built and configured Jenkins CI/CD pipelines for a Node.js application.
- Automated application testing, versioning, and Docker image publishing.
- Created reusable Jenkins Shared Library functions using Groovy.
- Provisioned AWS infrastructure using the AWS CLI.
- Configured networking, SSH access, security groups, and EC2 instances.
- Deployed containerized applications to AWS EC2 using Docker Compose.
- Automated remote deployments using Jenkins, SSH, and Bash scripts.
- Configured branch-based pipeline execution to separate testing from deployment.
- Integrated GitHub webhooks for automatic pipeline triggering.
- Managed deployment credentials using Jenkins credentials.

## Related Repository

[Jenkins Shared Library](https://github.com/Tea-naa/jenkins-shared-library) — Reusable Groovy functions used by this pipeline.

---

**Project completed as part of the TechWorld with Nana DevOps Bootcamp — Jenkins and AWS Services modules.**
