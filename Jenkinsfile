pipeline {
    agent any

    environment {
        DOCKER_USER = "rushimagar1"
        BRANCH = "${env.BRANCH_NAME}"
    }

    stages {

        stage('Show Branch') {
            steps {
                echo "Building branch: ${BRANCH}"
            }
        }

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
                branch 'BackEnd'
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
                branch 'FrontendNew'
            }
            steps {
                sh '''
                  docker build -t $DOCKER_USER/insurance-frontend:latest .
                  docker push $DOCKER_USER/insurance-frontend:latest
                '''
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                sh '''
                  docker compose down || true
                  docker compose up -d
                '''
            }
        }
    }
}
