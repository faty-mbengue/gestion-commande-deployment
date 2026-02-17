pipeline {
    agent { label 'agent-windows' }

    environment {
        // Variables d'environnement
        BACKEND_IMAGE = 'fatymbengue/gestion-commande-backend:latest'
        FRONTEND_IMAGE = 'fatymbengue/gestion-commande-frontend:latest'
        DEPLOY_DIR = 'C:\\dossier-agent\\deployment\\gestion-commande'
    }

    stages {
        stage('Checkout Deployment Repo') {
            steps {
                checkout scm
            }
        }

        stage('Pull Latest Images') {
            steps {
                bat """
                    echo Pulling latest images...
                    docker pull ${BACKEND_IMAGE}
                    docker pull ${FRONTEND_IMAGE}
                """
            }
        }

        stage('Deploy Application') {
            steps {
                bat """
                    echo Création du dossier de déploiement...
                    if not exist ${DEPLOY_DIR} mkdir ${DEPLOY_DIR}

                    echo Copie des fichiers...
                    copy docker-compose.yml ${DEPLOY_DIR}\\docker-compose.yml
                    copy nginx\\nginx.conf ${DEPLOY_DIR}\\nginx.conf
                    copy .env ${DEPLOY_DIR}\\.env 2>nul

                    echo Démarrage des conteneurs...
                    cd ${DEPLOY_DIR}
                    docker-compose down
                    docker-compose pull
                    docker-compose up -d
                    docker system prune -f
                """
            }
        }

        stage('Health Check') {
            steps {
                bat """
                    echo Attente du démarrage (10 secondes)...
                    timeout /t 10 /nobreak

                    echo Vérification du backend...
                    curl -f http://localhost:8085/api/health || echo Backend health check échoué

                    echo Vérification du frontend...
                    curl -f http://localhost || echo Frontend health check échoué
                """
            }
        }
    }

    post {
        success {
            echo "✅ Déploiement réussi sur agent-windows !"
        }
        failure {
            echo "❌ Échec du déploiement - Vérifie les logs ci-dessus"
        }
    }
}