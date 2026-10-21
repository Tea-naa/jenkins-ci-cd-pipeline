# Jenkins CI/CD Pipeline

A CI/CD pipeline for a Node.js application using Jenkins, Docker, GitHub, and Docker Hub.

The pipeline automates application testing, versioning, Docker image creation and publishing, and commits the updated application version back to GitHub. The application was also deployed as a Docker container on a DigitalOcean server.

## Pipeline

The Jenkins pipeline performs the following steps:

1. Increments the application version using npm
2. Installs dependencies and runs Jest tests
3. Builds a versioned Docker image
4. Pushes the image to Docker Hub
5. Commits the updated version back to GitHub

Example:

```text
GitHub
  ↓
Jenkins
  ↓
Increment Version
  ↓
Run Tests
  ↓
Build Docker Image
  ↓
Docker Hub
  ↓
Commit Updated Version to GitHub
```

## Jenkins Shared Library

Reusable pipeline logic was moved into a separate Jenkins Shared Library rather than keeping all implementation logic in the Jenkinsfile.

The shared library contains functions for:

- Version incrementing
- Running tests
- Building and pushing Docker images
- Committing version updates

This keeps the Jenkinsfile smaller and allows common CI/CD logic to be reused by other pipelines.

## Docker Deployment

The application is packaged as a Docker image and published to Docker Hub with the application version as the image tag.

Example:

```text
teanaa/jenkins-node-app:1.2.0
```

The published image was manually pulled and deployed as a Docker container on a DigitalOcean Ubuntu server.

## Tech Stack

- Jenkins
- Docker
- GitHub
- Docker Hub
- DigitalOcean
- Node.js
- npm
- Jest
- Groovy

## Project Structure

```text
.
├── app/
│   ├── Dockerfile
│   ├── package.json
│   ├── server.js
│   └── server.test.js
├── Jenkinsfile
└── README.md
```

## What I Practiced

This project gave me hands-on experience building a Jenkins CI/CD workflow, managing credentials securely in Jenkins, versioning application builds, publishing Docker images, deploying containers to a cloud server, and extracting reusable pipeline logic into a Jenkins Shared Library.