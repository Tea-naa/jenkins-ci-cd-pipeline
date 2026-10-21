pipeline {
    agent any
    tools {
        nodejs "node"
    }

    stages {
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