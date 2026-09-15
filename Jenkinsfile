pipeline {
    agent any

    environment {
        APP_DIR = '/home/ubuntu/fastapi-mysql-employee-crud'
    }

    stages {

        stage('Pull Latest Code') {
            steps {
                sh '''
                    cd $APP_DIR
                    git pull origin main
                '''
            }
        }

        stage('Create Environment Files') {
            steps {
                withCredentials([
                    string(credentialsId: 'app-env', variable: 'APP_ENV'),
                    string(credentialsId: 'backend-env', variable: 'BACKEND_ENV')
                ]) {
                    sh '''
                        printf '%s\\n' "$APP_ENV" > $APP_DIR/.env
                        printf '%s\\n' "$BACKEND_ENV" > $APP_DIR/backend/.env

                        chmod 600 $APP_DIR/.env
                        chmod 600 $APP_DIR/backend/.env
                    '''
                }
            }
        }

        stage('Stop Existing Containers') {
            steps {
                sh '''
                    cd $APP_DIR
                    docker compose down || true
                '''
            }
        }

        stage('Build and Start') {
            steps {
                sh '''
                    cd $APP_DIR
                    docker compose up -d --build
                '''
            }
        }

        stage('Verify') {
            steps {
                sh '''
                    cd $APP_DIR
                    docker compose ps
                '''
            }
        }
    }
}
