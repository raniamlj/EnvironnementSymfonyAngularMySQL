# Projet Fullstack Docker : Symfony + Angular + MySQL

## Description

Ce projet fournit un environnement de développement complet pour :

- Backend : Symfony API REST
- Frontend : Angular
- Base de données : MySQL
- Administration BD : phpMyAdmin
- Conteneurisation : Docker & Docker Compose

L’objectif est d’avoir un environnement identique pour tous les étudiants afin d’éviter les problèmes d’installation et de compatibilité.

---

# Structure du projet

```text
project/
│
├── docker-compose.yml
│
├── backend/
│   └── Dockerfile
│
├── frontend/
│   └── Dockerfile
│
└── README.md
```

---

# Technologies utilisées

| Technologie | Version |
|---|---|
| PHP | 8.2 |
| Symfony | 7 |
| Angular | 19 |
| Node.js | 20 |
| MySQL | 8 |
| Docker | Latest |

---

# Prérequis

Installer :

- Docker Desktop
- Git

Vérifier l’installation :

```bash
docker --version
docker compose version
git --version
```

---

# Démarrage du projet

Dans le dossier principal :

```bash
docker compose up --build -d
```

Vérifier les conteneurs :

```bash
docker ps
```

---

# Important

Les dossiers :

```text
backend/
frontend/
```

sont initialement vides.

Les projets Symfony et Angular doivent être créés manuellement après le démarrage des conteneurs Docker.

Les conteneurs restent actifs grâce à :

```yaml
command: tail -f /dev/null
```

Cela permet aux étudiants d’entrer dans les conteneurs et de créer les projets manuellement.

---

# Services disponibles

| Service | URL |
|---|---|
| Angular | http://localhost:4200 |
| Symfony API | http://localhost:8000 |
| phpMyAdmin | http://localhost:8081 |

---

# Informations MySQL

| Paramètre | Valeur |
|---|---|
| Host | mysql |
| Port | 3306 |
| Database | employee_db |
| User | symfony |
| Password | symfony |

Compte administrateur :

| Paramètre | Valeur |
|---|---|
| User | root |
| Password | root |

---

# Création du projet Symfony

Entrer dans le conteneur backend :

```bash
docker compose exec backend bash
```

Créer le projet Symfony :

```bash
composer create-project symfony/skeleton .
```

Installer Doctrine :

```bash
composer require symfony/orm-pack
```

Installer Maker Bundle :

```bash
composer require --dev symfony/maker-bundle
```

Installer CORS :

```bash
composer require nelmio/cors-bundle
```

Configurer la connexion MySQL dans :

```text
.env
```

Modifier :

```env
DATABASE_URL="mysql://symfony:symfony@mysql:3306/employee_db?serverVersion=8.0"
```

Créer la base :

```bash
php bin/console doctrine:database:create
```

Lancer le serveur Symfony :

```bash
symfony server:start --allow-http --no-tls --port=8000
```

Tester Symfony :

```text
http://localhost:8000
```

---

# Configuration CORS Symfony

Créer le fichier :

```text
config/packages/nelmio_cors.yaml
```

Contenu :

```yaml
nelmio_cors:
    defaults:
        allow_origin: ['http://localhost:4200']
        allow_methods: ['GET', 'POST', 'PUT', 'DELETE', 'OPTIONS']
        allow_headers: ['Content-Type', 'Authorization']

    paths:
        '^/':
            allow_origin: ['http://localhost:4200']
            allow_headers: ['Content-Type', 'Authorization']
            allow_methods: ['GET', 'POST', 'PUT', 'DELETE', 'OPTIONS']
```

---

# Création du projet Angular

Entrer dans le conteneur frontend :

```bash
docker compose exec frontend bash
```

Créer le projet Angular :

```bash
ng new .
```

Réponses recommandées :

```text
Would you like to add Angular routing? Yes
Which stylesheet format would you like to use? CSS
```

Installer les dépendances :

```bash
npm install
```

Lancer Angular :

```bash
ng serve --host 0.0.0.0 --poll=2000
```

Tester Angular :

```text
http://localhost:4200
```

---

# Commandes Docker utiles

## Voir les conteneurs

```bash
docker ps
```

## Voir les logs

```bash
docker compose logs -f
```

## Arrêter les conteneurs

```bash
docker compose down
```

## Supprimer les volumes

```bash
docker compose down -v
```

## Rebuild des images

```bash
docker compose build --no-cache
```

---

# Génération d’une API REST Symfony

Créer une entité :

```bash
php bin/console make:entity Employee
```

Créer la migration :

```bash
php bin/console make:migration
```

Exécuter la migration :

```bash
php bin/console doctrine:migrations:migrate
```

Créer un contrôleur API :

```bash
php bin/console make:controller EmployeeApiController
```

---

# Exemple Routes API

| Méthode | Route |
|---|---|
| GET | /api/employees |
| GET | /api/employees/{id} |
| POST | /api/employees |
| PUT | /api/employees/{id} |
| DELETE | /api/employees/{id} |

---

# phpMyAdmin

Accès :

```text
http://localhost:8081
```

Connexion :

```text
Serveur : mysql
Utilisateur : root
Mot de passe : root
```

---

# Arrêt complet du projet

```bash
docker compose down
```

Suppression complète :

```bash
docker compose down -v --rmi all
```

---

# Ressources utiles

- Symfony Documentation
- Angular Documentation
- Docker Documentation
- MySQL Documentation
- phpMyAdmin Documentation
