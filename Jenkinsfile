pipeline {

    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/omkarthakare/student-portfolio.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t student-portfolio:latest .'
            }
        }

        stage('Load Image into Minikube') {
            steps {
                sh '''
                    docker save student-portfolio:latest | docker exec -i minikube ctr -n k8s.io images import -
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                    kubectl apply -f deployment.yaml
                    kubectl apply -f service.yaml
                    kubectl rollout restart deployment/student-portfolio
                    kubectl rollout status deployment/student-portfolio
                '''
            }
        }

    }
}