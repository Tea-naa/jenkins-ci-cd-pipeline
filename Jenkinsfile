@Library('jenkins-shared-library') _

pipeline {
    agent any

    tools {
        nodejs "node"
    }

    stages {
    stage('Increment Version') {
        when {
            branch 'main'
        }
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
    when {
        branch 'main'
    }
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

        stage('Deploy to EC2') {
    when {
        branch 'main'
    }
    steps {
        script {
            def version = sh(
                script: "node -p \"require('./app/package.json').version\"",
                returnStdout: true
            ).trim()

            def IMAGE_NAME = "teanaa/jenkins-node-app:${version}"
            def shellCmd = "bash ./server-cmds.sh ${IMAGE_NAME}"
            def ec2Instance = "ec2-user@3.137.220.26"

            sshagent(['ec2-server-key']) {
                sh "scp -o StrictHostKeyChecking=no server-cmds.sh ${ec2Instance}:/home/ec2-user"
                sh "scp -o StrictHostKeyChecking=no docker-compose.yaml ${ec2Instance}:/home/ec2-user"
                sh "ssh -o StrictHostKeyChecking=no ${ec2Instance} ${shellCmd}"
            }
        }
    }
}

        stage('Commit Version to GitHub') {
    when {
        expression { false }
    }
    steps {
        dir('app') {
            commitVersionUpdate()
        }
    }
}
}
}
