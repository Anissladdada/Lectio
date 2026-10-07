# Lectio

## Description
Lectio est une application de gestion de bibliothèque.
Elle permet de consulter le catalogue, d'emprunter des livres et de gérer les retards.
Le projet est réalisé dans le cadre du cours NoSQL Databases.

## Architecture
L'application est composée de trois services :
- **Service Catalogue** : gère les livres et les exemplaires disponibles
- **Service Prêt** : gère les emprunts et les retours
- **Service Pénalité** : calcule les pénalités en cas de retard

Chaque service possède sa propre base PostgreSQL pour son état officiel.
Les services communiquent par des événements, transportés par des listes Redis.
Une base MongoDB sert aux vues de lecture (projections).
Le tout est lancé avec Docker Compose.

## Agrégats
Un agrégat est une frontière métier transactionnelle.
Chaque service modifie uniquement son propre agrégat.

- **Livre** (service Catalogue) : titre, auteur, exemplaires disponibles.
  Le nombre d'exemplaires disponibles ne peut jamais passer sous zéro.
- **Prêt** (service Prêt) : livre, membre, date de retour, statut.
  Un prêt est créé ou clôturé en une seule transaction.
- **Pénalité** (service Pénalité) : copie du titre, jours de retard, montant.
  Une seule pénalité est créée par prêt en retard.

## Événements
PretEnRetard v1 contient pretId, livreId, titre, membreId et jours de retard.
Le service Pénalité le consomme et crée une pénalité.
Un index unique sur eventId évite les doublons.
Le service Pénalité garde une copie du titre pour répondre sans appeler le service Catalogue.
Si le titre change, un événement corrigé est publié. Le service Pénalité ne devine jamais la valeur.

## Projection NoSQL
prets_par_membre : prêts d'un membre, triés par date de retour.
Elle sert à la page "Mes emprunts".
Elle est construite à partir des événements et peut être reconstruite.
Ce n'est pas la source de vérité.

## Choix CAP
- Emprunter un livre : CP. En cas de doute sur le stock, l'emprunt est refusé.
- Consulter le catalogue : AP. La liste peut avoir un léger retard.

## Technologies
PostgreSQL, Redis, MongoDB, Docker, Docker Compose, Git.
Le langage des services reste à confirmer.

## Organisation
- Équipe : 1 personne (Anis Laddada)
- Dépôt : GitHub
- Date de remise : 8 novembre

## Avancement
- [x] Choix du sujet
- [x] Création du dépôt
- [x] Descriptif et agrégats
- [ ] Service Catalogue
- [ ] Service Prêt
- [ ] Service Pénalité
- [ ] Projection MongoDB
- [ ] Docker Compose
