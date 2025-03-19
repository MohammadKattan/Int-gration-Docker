# Intégration-Docker
## Déploiement

### 1. Méthode avec une image Docker locale
- Cette méthode utilise une image Docker locale.
- Elle nécessite **deux étapes** : `build` et `deploy`.
- Commit correspondant : **debug nom de fichier**.

### 2. Méthode avec une image Docker Hub
- Cette méthode utilise une image déjà publiée sur Docker Hub.
- Elle ne nécessite **que l'étape deploy**.
- Commit correspondant : **test docker hub**.

### Problème rencontré
Lors du déploiement avec un **self-hosted runner**, une erreur **"cannot execute binary file"** bloque la création du cluster Kind.
