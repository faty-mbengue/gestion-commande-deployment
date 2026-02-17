pipeline {
    agent any

    environment {
        DOCKER_COMPOSE = "docker-compose"
    }

    stages {

        stage('Checkout Deployment Repo') {
            steps {
                checkout scm
            }
        }

        stage('Pull Latest Images') {
            steps {
                sh '''
                    docker pull fatymbengue/gestion-commande-backend:latest
                    docker pull fatymbengue/gestion-commande-frontend:latest
                '''
            }
        }

        stage('Deploy Application') {
            steps {
                sh '''
                    docker-compose down
                    docker-compose up -d
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Déploiement continu effectué avec succès.'
        }
        failure {
            echo '❌ Erreur lors du déploiement.'
        }
    }
}
