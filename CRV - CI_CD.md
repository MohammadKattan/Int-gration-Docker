# TME CI/CD (Intégration Continue / Déploiement Continu)

## Objectifs :
Ce TME vous permettra de comprendre et pratiquer les étapes fondamentales d’un pipeline CI/CD en vous appuyant sur les acquis précédents en Docker et Kubernetes.

## Prérequis :
- Avoir réalisé les TME précédents sur Docker et Kubernetes.
- Avoir initialisé un dépôt Git local et distant.

## Étape 1 : Gestion du code avec Git

1. **Initialiser Git (si nécessaire)** :
```bash
git init
git add .
git commit -m "Initial commit"
```

2. **Créer et lier le dépôt distant (si nécessaire)** :
```bash
git remote add origin <url_de_votre_repo>
git branch -M main
git push -u origin main
```

3. **Effectuer des modifications et pousser les changements** :
```bash
git add .
git commit -m "Description des modifications"
git push origin main
```

## Étape 2 : Intégration Continue avec GitHub Actions

### Création du workflow CI :

1. **Créer un dossier `.github/workflows` dans votre projet :**
```bash
mkdir -p .github/workflows
```

2. **Créer un fichier nommé `ci.yml` dans ce dossier :**
```bash
touch .github/workflows/ci.yml
```

3. **Éditer le fichier `ci.yml` avec le contenu suivant :**

```yaml
name: CI Pipeline

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        name: Checkout du code source

      - name: Construire l'image Docker
        run: |
          docker build -t yourusername/hello-world-node:${{ github.sha }} .

      - name: Connexion à Docker Hub
        run: echo ${{ secrets.DOCKER_PASSWORD }} | docker login -u ${{ secrets.DOCKER_USERNAME }} --password-stdin

      - name: Pousser l'image Docker
        run: |
          docker tag yourusername/hello-world-node:${{ github.sha }} yourusername/hello-world-node:latest
          docker push yourusername/hello-world-node:latest
```

### Activation du workflow :
- Une fois le fichier créé et poussé sur GitHub, GitHub Actions lancera automatiquement ce workflow à chaque push sur la branche `main`.
- Consultez l'onglet **Actions** sur GitHub pour suivre les exécutions et vérifier l'état de vos intégrations continues.

## Étape 3 : Déploiement Continu avec Kubernetes

1. **Créer un fichier de déploiement Kubernetes (`deployment.yml`) :**
Utilisez celui réalisé lors des TME précédents.

2. **Créer un service Kubernetes (`service.yml`) :**
Utilisez celui réalisé lors des TME précédents.

3. **Automatiser le déploiement avec GitHub Actions** :
Ajouter une étape de déploiement au pipeline GitHub Actions dans `ci.yml` :

```yaml
  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: azure/setup-kubectl@v3
        with:
          version: 'latest'
      - name: Déployer sur Kubernetes
        run: |
          kubectl set image deployment/hello-world-node hello-world-node=yourusername/hello-world-node:latest --record
        env:
          KUBECONFIG: ${{ secrets.KUBECONFIG }}
```

## Étape 4 : Tests et Vérification

- Vérifier le déploiement :
```bash
kubectl get pods
kubectl get service hello-world-service
```

- Testez l'application via le navigateur à l’adresse IP fournie par le service LoadBalancer.

## Étape 5 : Nettoyage

Nettoyer les ressources Kubernetes :
```bash
kubectl delete deployment hello-world-node
kubectl delete service hello-world-service
```

---

Vous avez désormais un pipeline complet CI/CD pour vos futures applications conteneurisées.

