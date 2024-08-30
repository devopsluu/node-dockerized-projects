pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Test') {
            steps {
                // Install npm non-interactively
                sh 'sudo apt-get update'
                sh 'sudo DEBIAN_FRONTEND=noninteractive apt-get install -y npm'
                sh 'npm test'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Build Image') {
            steps {
                sh 'docker build -t my-node-app:1.0 .'
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker_cred', passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]) {
                    sh 'docker login -u $DOCKERHUB_USERNAME -p $DOCKERHUB_PASSWORD'
                    sh 'docker tag my-node-app:1.0 kavi1122/my-node-app:1.0'
                    sh 'docker push kavi1122/my-node-app:1.0'
                    sh 'docker logout'
                }
            }
        }
    }
}
