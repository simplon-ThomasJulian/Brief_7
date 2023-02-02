---
slideOptions:
  transition: slide
  theme: moon
---

# Executive Summary Brief 7

---

## Déploiement de la Pipeline
On utilise Azure DevOps pour construire et déployer le Pipeline

---

## Point d'entrée

On doit créer des points de connexions:  
- cluster --> pipeline 
- repository Github --> pipeline

---

## Création d'un trigger

- Trigger lié au push sur le main
- Trigger lié à une programmation pour tourner toutes les heures

---

## Récupérer les variables

- Version déployée dans le cluster, on récupère l'output de Get Deployments en Json puis on le parse
- Curl pour récupérér la version sur Dockerhub, parser pour avoir le même format pour comparer
- On les places dans deux variables réutilisables dans une autre task du pipeline.

---

## Clonage du repo Github

- On clone le repo github pour modifier directement la version dans le fichier de déploiement de l'application

---

## Création de la condition
- Nouveau apply si la condition "ne" entre les deux versions est vrai
- sinon pas de nouveau apply

---

# Difficultés rencontrées
- Utilisations des variables dans plusieurs tasks
- Parser correctement les versions pour la comparaison
- Gérer tous les points de connections entre la pipeline/cluster/repository

---

### Lien vers le Github

https://github.com/simplon-ThomasJulian/Brief_7

---