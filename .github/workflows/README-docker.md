# Workflows Docker pour le déploiement du Chatbot Orange

Ce dossier contient les workflows GitHub Actions pour construire, pousser et déployer les images Docker des trois composants du système de chatbot Orange.

## Workflows disponibles

1. **build-push-bot-site-demo.yml** - Construit et pousse l'image Docker du frontend React (Bot-site-Demo)
2. **build-push-directline-service.yml** - Construit et pousse l'image Docker du service intermédiaire Node.js (Directline-service)
3. **build-push-chatbot-gateway.yml** - Construit et pousse l'image Docker du backend .NET (ChatbotGatewayOrange)
4. **docker-compose-deploy.yml** - Déploie les trois services ensemble sur une VM Azure via Docker Compose

## Configuration requise

### 1. Secrets GitHub

Configurez les secrets suivants dans votre dépôt GitHub (Settings > Secrets and variables > Actions > New repository secret) :

#### Pour l'authentification au registre de conteneurs
- `REGISTRY_USERNAME` : Nom d'utilisateur pour votre registre de conteneurs
- `REGISTRY_PASSWORD` : Mot de passe ou token pour votre registre de conteneurs

#### Pour le frontend (Bot-site-Demo)
- `REACT_APP_BOT_DOMAIN` : URL du service DirectLine
- `REACT_APP_BOT_TOKEN` : Token DirectLine

#### Pour le service DirectLine (Directline-service)
- `BOT_URL` : URL du backend
- `TUNNEL_URL` : URL du service DirectLine

#### Pour le backend (ChatbotGatewayOrange)
- `RASA_ENDPOINT` : URL du service Rasa NLU
- `SQL_CONNECTION_STRING` : Chaîne de connexion à la base de données Azure SQL

#### Pour le déploiement Docker Compose
- `VM_IP` : Adresse IP de la machine virtuelle
- `VM_USER` : Nom d'utilisateur SSH
- `SSH_PRIVATE_KEY` : Clé SSH privée pour se connecter à la VM
- `DIRECTLINE_HOST` : Nom d'hôte du service DirectLine
- `DIRECTLINE_TOKEN` : Token DirectLine

### 2. Configuration du registre de conteneurs

Modifiez la variable `REGISTRY` dans chaque workflow pour pointer vers votre registre de conteneurs :

```yaml
env:
  REGISTRY: 'votre-registre.azurecr.io'  # Remplacer par votre registre
```

Options courantes :
- Azure Container Registry : `votreacr.azurecr.io`
- Docker Hub : `docker.io/votreusername`
- GitHub Container Registry : `ghcr.io/votreusername`

## Utilisation des workflows

### Construction et push des images Docker

Les workflows de construction se déclenchent automatiquement lorsque vous poussez des modifications sur les branches `main` ou `master` dans les dossiers correspondants.

Vous pouvez également les déclencher manuellement :
1. Accédez à l'onglet "Actions" de votre dépôt GitHub
2. Sélectionnez le workflow que vous souhaitez exécuter
3. Cliquez sur "Run workflow"

### Déploiement avec Docker Compose

Le workflow de déploiement Docker Compose est conçu pour être déclenché manuellement uniquement :
1. Accédez à l'onglet "Actions" de votre dépôt GitHub
2. Sélectionnez "Deploy All Services with Docker Compose"
3. Cliquez sur "Run workflow"
4. Sélectionnez l'environnement (staging ou production)
5. Cliquez sur "Run workflow"

## Prérequis pour la VM de déploiement

La VM Azure qui recevra le déploiement doit avoir :
1. Docker installé
2. Docker Compose installé
3. L'utilisateur SSH configuré avec les permissions Docker
4. Les ports nécessaires ouverts (3000, 3978, 5000)

## Structure des images Docker

Chaque application doit avoir un Dockerfile valide à la racine de son dossier :
- `Bot-site-Demo/Dockerfile`
- `Directline-service/Dockerfile`
- `ChatbotGatewayOrange/Dockerfile`

## Personnalisation

Vous pouvez personnaliser les workflows en modifiant les fichiers YAML :

- Changez les noms d'images dans les variables d'environnement `IMAGE_NAME`
- Modifiez les tags des images selon vos besoins
- Ajustez les arguments de build pour chaque image

## Dépannage

En cas d'échec de construction ou de déploiement :

1. Vérifiez les journaux d'exécution dans l'onglet Actions
2. Assurez-vous que tous les secrets sont correctement configurés
3. Vérifiez que les Dockerfiles sont valides et fonctionnels
4. Confirmez que le registre de conteneurs est accessible
5. Vérifiez la connectivité SSH vers la VM de déploiement

Pour plus d'informations sur Docker et GitHub Actions, consultez :
- [Documentation Docker](https://docs.docker.com/)
- [Documentation GitHub Actions](https://docs.github.com/en/actions)
- [Docker Compose](https://docs.docker.com/compose/) 