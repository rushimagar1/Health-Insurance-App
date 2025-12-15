pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Backend') {
            when {
                branch 'BackEnd'
            }
            steps {
                dir('BackEnd/HealthInsurance') {
                    sh 'docker build -t rushimagar1/insurance-backend:latest .'
                    sh 'docker push rushimagar1/insurance-backend:latest'
                }
            }
        }

        stage('Build Frontend') {
            when {
                branch 'FrontendNew'
            }
            steps {
                dir('FrontEnd') {
                    sh 'docker build -t rushimagar1/insurance-frontend:latest .'
                    sh 'docker push rushimagar1/insurance-frontend:latest'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f HealthInsurance/app-deploy.yaml'
            }
        }
    }
}
