# Workflows GitHub Actions pour le déploiement du Chatbot Orange

Ce dossier contient les workflows GitHub Actions pour déployer automatiquement les trois composants du système de chatbot Orange sur Azure.

## Workflows disponibles

1. **deploy-bot-site-demo.yml** - Déploie le frontend React (Bot-site-Demo)
2. **deploy-directline-service.yml** - Déploie le service intermédiaire Node.js (Directline-service)
3. **deploy-chatbot-gateway.yml** - Déploie le backend .NET (ChatbotGatewayOrange)

## Configuration requise

### 1. Secrets GitHub

Configurez les secrets suivants dans votre dépôt GitHub (Settings > Secrets and variables > Actions > New repository secret) :

#### Pour tous les workflows
- `AZURE_CREDENTIALS` : JSON d'authentification Service Principal Azure

#### Pour Bot-site-Demo
- `REACT_APP_BOT_DOMAIN` : URL du service DirectLine (ex: https://directline-service.azurewebsites.net/v3/directline)
- `REACT_APP_BOT_TOKEN` : Token DirectLine
- `AZURE_STATIC_WEB_APPS_API_TOKEN` : Token d'API pour Azure Static Web Apps (si vous utilisez cette option)

#### Pour Directline-service
- `BOT_URL` : URL du backend (ex: https://chatbot-gateway-orange.azurewebsites.net)
- `TUNNEL_URL` : URL du service DirectLine (ex: https://directline-service.azurewebsites.net)

#### Pour ChatbotGatewayOrange
- `RASA_ENDPOINT` : URL du service Rasa NLU
- `SQL_CONNECTION_STRING` : Chaîne de connexion à la base de données Azure SQL

### 2. Création du Service Principal Azure

Pour obtenir les identifiants Azure (`AZURE_CREDENTIALS`), exécutez la commande suivante avec Azure CLI :

```bash
az ad sp create-for-rbac --name "GitHubActionsChatbot" --role contributor \
                         --scopes /subscriptions/{subscription-id}/resourceGroups/{resource-group} \
                         --sdk-auth
```

Le résultat JSON doit être copié et stocké comme secret GitHub `AZURE_CREDENTIALS`.

## Utilisation des workflows

### Déclenchement automatique

Les workflows se déclenchent automatiquement lorsque vous poussez des modifications sur les branches `main` ou `master` dans les dossiers correspondants :
- `Bot-site-Demo/**` pour le frontend
- `Directline-service/**` pour le service DirectLine
- `ChatbotGatewayOrange/**` pour le backend

### Déclenchement manuel

Vous pouvez également déclencher manuellement chaque workflow :
1. Accédez à l'onglet "Actions" de votre dépôt GitHub
2. Sélectionnez le workflow que vous souhaitez exécuter
3. Cliquez sur "Run workflow"
4. Sélectionnez la branche et cliquez sur "Run workflow"

## Personnalisation

Vous pouvez personnaliser les workflows en modifiant les fichiers YAML :

- Changez les noms d'App Service dans les variables d'environnement `APP_NAME`
- Modifiez le groupe de ressources dans `RESOURCE_GROUP`
- Ajustez les versions de runtime (.NET, Node.js) selon vos besoins

## Dépannage

En cas d'échec de déploiement :

1. Vérifiez les journaux d'exécution dans l'onglet Actions
2. Assurez-vous que tous les secrets sont correctement configurés
3. Vérifiez que les ressources Azure existent et sont accessibles
4. Confirmez que le Service Principal a les autorisations nécessaires

Pour plus d'informations sur GitHub Actions, consultez la [documentation officielle](https://docs.github.com/en/actions). 