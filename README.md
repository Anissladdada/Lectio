# TaskBoard

## Description
TaskBoard est une application web de gestion de tâches.
Elle permet de créer, afficher et supprimer des tâches.
Le projet est réalisé seul, dans le cadre du cours.

## Architecture
L'application est composée de trois services :
- **frontend** : interface web pour l'utilisateur
- **api** : API REST qui gère la logique métier (FastAPI)
- **db** : base de données PostgreSQL qui stocke les tâches

Les services communiquent ainsi : frontend → api → db.
Ils sont lancés ensemble avec Docker Compose.

## Technologies
Python, FastAPI, PostgreSQL, Docker, Docker Compose, Git.

## Lancer le projet
    docker compose up --build

## Organisation
- Équipe : 1 personne (Anis Laddada)
- Dépôt : GitHub

## Avancement
- [x] Choix du sujet
- [x] Création du dépôt
- [ ] Service api
- [ ] Service db
- [ ] Service frontend
- [ ] Docker Compose