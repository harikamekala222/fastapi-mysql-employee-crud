pipeline {
    agent any

    stages {

        stage('Create Environment Files') {
            steps {
                withCredentials([
                    string(credentialsId: 'app-env', variable: 'APP_ENV'),
                    string(credentialsId: 'backend-env', variable: 'BACKEND_ENV')
                ]) {
                    sh '''
                        printf '%s\\n' "$APP_ENV" > .env
                        printf '%s\\n' "$BACKEND_ENV" > backend/.env

                        chmod 600 .env
                        chmod 600 backend/.env

                        set -a
                        . ./.env
                        set +a

                        echo "Environment files created successfully"
                    '''
                }
            }
        }

        stage('Stop Existing Containers') {
            steps {
                sh '''
                    docker compose down || true

                    docker stop employee_mysql employee_backend employee_frontend 2>/dev/null || true
                    docker rm employee_mysql employee_backend employee_frontend 2>/dev/null || true
                '''
            }
        }

        stage('Build and Start') {
            steps {
                withCredentials([
                    string(credentialsId: 'app-env', variable: 'APP_ENV'),
                    string(credentialsId: 'backend-env', variable: 'BACKEND_ENV')
                ]) {
                    sh '''
                        printf '%s\\n' "$APP_ENV" > .env
                        printf '%s\\n' "$BACKEND_ENV" > backend/.env

                        chmod 600 .env
                        chmod 600 backend/.env

                        set -a
                        . ./.env
                        set +a

                        docker compose up -d --build
                    '''
                }
            }
        }

        stage('Verify') {
            steps {
                sh '''
                    docker compose ps
                '''
            }
        }
    }
}
