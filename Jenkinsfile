pipeline {
    agent any

    environment {
        BRANCH = "${env.GIT_BRANCH}".replace("origin/", "")
        DOCKER_USER = "rushimagar1"
    }

    stages {

        stage('Show Branch') {
            steps {
                echo "Building branch: ${BRANCH}"
            }
        }

        stage('Build Backend') {
            when {
                expression { BRANCH == 'BackEnd' }
            }
            steps {
                echo "Backend build started"
                sh '''
                  cd BackEnd/HealthInsurance
                  mvn clean package -DskipTests
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
                  cd FrontEnd
                  docker build -t $DOCKER_USER/insurance-frontend:latest .
                  docker push $DOCKER_USER/insurance-frontend:latest
                '''
            }
        }

        stage('Deploy Containers') {
            steps {
                echo "Deploying application"
                sh '''
                  docker compose up -d
                '''
            }
        }
    }
}
