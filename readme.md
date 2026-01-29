# 📝 Déploiement Kubernetes (K3s) - Bot Discord PLC

Ce dépôt contient les manifestes "Infrastructure as Code" pour déployer le **Bot Discord du groupe Paul-Louis Courier** sur un cluster **K3s**.

L'architecture met en œuvre un déploiement automatisé avec gestion du routage via Traefik (pour l'interface web/webhooks) et mise à jour continue via Keel.

## 🏗️ Architecture Technique

Le déploiement orchestre les composants suivants :

* **Image :** `ghcr.io/lycee-paul-louis-courier-bts-sio/discord-bot-plc:latest`
* **Auto-déploiement :** Keel (Polling toutes les 5 minutes pour détecter les nouvelles versions)

## 📂 Structure du dépôt

* [`namespace.yaml`](namespace.yaml) : Définition du namespace `discord-bot`
* [`secret-bot-plc.yaml.example`](secret-bot-plc.yaml.example) : Modèle de configuration des secrets (Token, API Keys)
* [`deployment.yaml`](deployment.yaml) : Deployment et Service de l'application

## 🚀 Prérequis

* Un cluster Kubernetes fonctionnel (testé sur K3s)
* Traefik installé comme Ingress Controller
* Keel installé pour l'auto-déploiement
* Token Discord valide

## 🛠️ Installation

1.  **Cloner le dépôt :**
    ```bash
    git clone https://github.com/FireToak/k3s-deployment_bot-plc.git
    cd k3s-deployment_bot-plc
    ```

2.  **Créer le namespace :**
    ```bash
    kubectl apply -f namespace.yaml
    ```

3.  **Configurer les Secrets :**
    Créez le fichier de secrets à partir de l'exemple, insérez votre Token Discord et vos clés API, puis appliquez-le.
    ```bash
    cp secret-bot-plc.yaml.example secret-bot-plc.yaml
    nano secret-bot-plc.yaml  # Éditez le fichier avec vos identifiants
    kubectl apply -f secret-bot-plc.yaml
    ```

4.  **Déployer l'application :**
    ```bash
    kubectl apply -f deployment.yaml
    ```

5.  **Vérification :**
    ```bash
    kubectl get pods -n discord-bot
    kubectl get svc -n discord-bot
    ```

## ⚙️ Configuration

### Ressources allouées
- **Limits :** 512Mi RAM / 500m CPU
- **Requests :** 125Mi RAM / 25m CPU

### Politique Keel
- **Policy :** `force` (force la mise à jour)
- **Trigger :** `poll` (vérification périodique)
- **Schedule :** `@every 5m` (toutes les 5 minutes)

## 📦 À propos du bot

Ce dépôt contient uniquement les manifestes de déploiement Kubernetes. Le code source du bot se trouve dans le dépôt [bot-discord-plc](https://github.com/lycee-paul-louis-courier-bts-sio/discord-bot-plc).

## 👤 Auteur

- **Louis MEDO** - *Passionné par l'administration système ❤️* | [Linkedin](https://www.linkedin.com/in/louismedo/) | [Portfolio](https://louis.loutik.fr/) | [GitHub](https://github.com/FireToak)