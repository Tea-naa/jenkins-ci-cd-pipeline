pipeline {
    agent any

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