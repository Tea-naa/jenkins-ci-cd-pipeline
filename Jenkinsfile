pipeline {
    agent any

    tools {
        nodejs "node"
    }

    stages {
        stage('Increment Version') {
            steps {
                dir('app') {
                    sh 'npm version minor --no-git-tag-version'
                }
            }
        }

        stage('Test') {
            steps {
                dir('app') {
                    sh 'npm install'
                    sh 'npm test'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('app') {
                    script {
                        def version = sh(
                            script: "node -p \"require('./package.json').version\"",
                            returnStdout: true
                        ).trim()

                        sh "docker build -t teanaa/jenkins-node-app:${version} ."
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                dir('app') {
                    script {
                        def version = sh(
                            script: "node -p \"require('./package.json').version\"",
                            returnStdout: true
                        ).trim()

                        withCredentials([usernamePassword(
                            credentialsId: 'docker-hub',
                            usernameVariable: 'DOCKER_USER',
                            passwordVariable: 'DOCKER_PASS'
                        )]) {
                            sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
                            sh "docker push teanaa/jenkins-node-app:${version}"
                        }
                    }
                }
            }
        }
    }
}