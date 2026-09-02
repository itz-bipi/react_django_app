pipeline {

    agent any

    environment {
        APP_NAME = "react-django-app"
    }

    stages {
        stage('Verify Environment') {
            steps {
                echo 'Checking Jenkins environment...'

                sh '''
                    whoami
                    docker --version
                    docker compose version
                '''
            }
        }

        stage('Validate Docker Compose') {
            steps {
                echo 'Validating Docker Compose configuration...'

                sh '''
                    docker compose config
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'

                sh '''
                    docker compose build
                '''
            }
        }

        stage('Test Application') {
            steps {
                echo 'Running Django tests...'

                sh '''
                    docker compose run --rm web python manage.py test
                '''
            }
        }

        stage('Deploy Application') {
            steps {
                echo 'Deploying application...'

                sh '''
                    docker compose down
                    docker compose up -d
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                echo 'Checking running containers...'

                sh '''
                    docker compose ps
                '''
            }
        }
    }

    post {

        success {
            echo "Deployment of ${APP_NAME} completed successfully!"
        }

        failure {
            echo "Deployment of ${APP_NAME} failed!"
        }

        always {
            echo "Jenkins Pipeline execution completed."
        }
    }
}