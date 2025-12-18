pipeline {
    agent any

    environment {
        DOCKER_USER = "rushimagar1"
        BRANCH = "${env.GIT_BRANCH}".replace("origin/", "")
    }

    stages {

        stage('Show Branch') {
            steps {
                echo "Building branch: ${BRANCH}"
                sh 'ls'
            }
        }

        stage('Build Backend') {
            when {
                expression { BRANCH == 'BackEnd' }
            }
            steps {
                echo "Backend build started"
                sh '''
                  cd HealthInsurance
                  docker build -t $DOCKER_USER/insurance-backend:latest .
                  docker push $DOCKER_USER/insurance-backend:latest
                '''
            }
        }

        stage('Build Frontend') {
            when {
                expression { BRANCH == 'FrontendNew' }
            }
            steps {
                echo "Frontend build started"
                sh '''
                  docker build -t $DOCKER_USER/insurance-frontend:latest .
                  docker push $DOCKER_USER/insurance-frontend:latest
                '''
            }
        }

        stage('Deploy Containers') {
            when {
                expression { BRANCH == 'BackEnd' }
            }
            steps {
                echo "Deploying application"
                sh '''
                  docker compose down || true
                  docker compose up -d
                '''
            }
        }
    }
}
