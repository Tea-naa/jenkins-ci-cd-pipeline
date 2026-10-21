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
    }
}