# Gestion Commande - Deployment Repository

Ce dépôt contient les fichiers nécessaires au déploiement continu (CD) de l'application Gestion Commande.

## Contenu

- docker-compose.yml : Orchestration des conteneurs
- Jenkinsfile : Pipeline de déploiement automatique
- .env : Variables d’environnement
- nginx.conf : Configuration serveur web

## Déploiement manuel

docker-compose up -d

## Déploiement automatique

Le déploiement est exécuté automatiquement via Jenkins après validation du pipeline CI.
