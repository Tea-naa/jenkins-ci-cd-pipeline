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
        
        stage ('Test'){
            steps {
                dir('app') {
                    sh 'npm install'
                    sh 'npm test'
                }
            }
        }
    }
}