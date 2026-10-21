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

        stage('Commit Version to GitHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'github-credentials',
                    usernameVariable: 'GITHUB_USER',
                    passwordVariable: 'GITHUB_TOKEN'
                )]) {
                    sh '''
                        git config user.name "Jenkins"
                        git config user.email "jenkins@local"

                        git add app/package.json app/package-lock.json
                        git commit -m "Update app version"

                        git push https://$GITHUB_USER:$GITHUB_TOKEN@github.com/Tea-naa/jenkins-ci-cd-pipeline.git HEAD:main
                    '''
                }
            }
        }
    }
}