pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                bat 'docker build -t mkaur18/devops_week12:latest .'
            }
        }

        stage('Docker Login and Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_TOKEN'
                )]) {
                    bat 'echo %DOCKER_TOKEN%| docker login -u %DOCKER_USER% --password-stdin'
                    bat 'docker push mkaur18/devops_week12:latest'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat 'kubectl apply -f deployment.yaml'
                bat 'kubectl apply -f service.yaml'
                bat 'kubectl rollout restart deployment/week12-deployment'
                bat 'kubectl rollout status deployment/week12-deployment --timeout=120s'
            }
        }
    }

    post {
        success { echo 'Week 12 deployment completed successfully!' }
        failure { echo 'Pipeline failed. Check the logs.' }
    }
}