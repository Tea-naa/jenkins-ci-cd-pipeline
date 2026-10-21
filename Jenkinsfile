@Library('jenkins-shared-library') _

pipeline {
    agent any

    tools {
        nodejs "node"
    }

    stages {
        stage('Increment Version') {
            steps {
                dir('app') {
                    incrementVersion()
                }
            }
        }

        stage('Test') {
            steps {
                dir('app') {
                    runTests()
                }
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                dir('app') {
                    script {
                        def version = sh(
                            script: "node -p \"require('./package.json').version\"",
                            returnStdout: true
                        ).trim()

                        buildAndPushDockerImage(
                            'teanaa/jenkins-node-app',
                            version
                        )
                    }
                }
            }
        }

        stage('Commit Version to GitHub') {
            steps {
                dir('app') {
                    commitVersionUpdate()
                }
            }
        }
    }
}