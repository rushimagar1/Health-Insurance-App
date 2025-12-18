pipeline {
    agent any

    environment {
        DOCKER_USER = "rushimagar1"
        BRANCH = "${env.GIT_BRANCH}".replace("origin/", "")
    }

    stages {

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USERNAME',
                    passwordVariable: 'DOCKER_PASSWORD'
                )]) {
                    sh 'echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin'
                }
            }
        }

        stage('Build Backend Image') {
            when {
                expression { BRANCH == 'BackEnd' }
            }
            steps {
                sh '''
                  docker build -t $DOCKER_USER/insurance-backend:latest ./HealthInsurance
                  docker push $DOCKER_USER/insurance-backend:latest
                '''
            }
        }

        stage('Build Frontend Image') {
            when {
                expression { BRANCH == 'FrontendNew' }
            }
            steps {
                sh '''
                  docker build -t $DOCKER_USER/insurance-frontend:latest .
                  docker push $DOCKER_USER/insurance-frontend:latest
                '''
            }
        }

        stage('Deploy with Docker Compose') {
            when {
                expression { BRANCH == 'BackEnd' }
            }
            steps {
                sh '''
                  docker compose down || true
                  docker compose up -d --build
                '''
            }
        }
    }
}
