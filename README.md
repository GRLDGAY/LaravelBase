# LaravelBase

## Présentation

Ce dépôt constitue un **environnement de développement prêt à accueillir des projets Laravel**.

Il utilise **GitHub Codespaces** et Docker afin de disposer d'un environnement de développement Web complet dans le cloud.

Aucune installation locale de PHP, Composer, Apache ou MySQL n'est nécessaire.

> **Laravel n'est pas installé ni paramétré dans ce dépôt modèle.**

Chaque dépôt créé à partir de `LaravelBase` peut accueillir son propre projet Laravel.

---

# Architecture de l'environnement

Le Codespace utilise trois conteneurs Docker :

```text
GitHub Codespace
│
├── app
│   ├── Apache
│   ├── PHP 8.4
│   ├── Composer
│   ├── PDO MySQL
│   └── projetLaravel
│
├── mysql
│   └── MySQL 8.4
│
└── phpmyadmin
    └── Interface Web de gestion MySQL
```

Les conteneurs communiquent grâce au réseau Docker interne.

---

# Conteneur `app`

Le conteneur `app` constitue l'environnement principal de développement.

VS Code est connecté directement à ce conteneur.

Il contient :

- Apache ;
- PHP 8.4 ;
- Composer ;
- Git ;
- PDO MySQL ;
- un terminal Linux.

Le dossier de travail est :

```text
/workspace/projetLaravel
```

Ce dossier est destiné à accueillir le projet Laravel.

---

# Serveur Web Apache

Apache est installé et démarré automatiquement dans le conteneur `app`.

Le dossier Web utilisé par Apache est :

```text
/workspace/projetLaravel/public
```

Il correspond au futur dossier `public` de Laravel.

L'application Web est accessible depuis le port :

```text
8000
```

Le fonctionnement est le suivant :

```text
Navigateur
    │
    ▼
Port 8000 du Codespace
    │
    ▼
Port 80 du conteneur app
    │
    ▼
Apache
    │
    ▼
/workspace/projetLaravel/public
```

Il n'est donc pas nécessaire de lancer :

```bash
php artisan serve
```

Apache assure automatiquement le service Web.

---

# PHP et PDO MySQL

PHP 8.4 est installé dans le conteneur `app`.

Le pilote :

```text
pdo_mysql
```

est également installé automatiquement.

Il permet à PHP et Laravel de communiquer avec le serveur MySQL.

Sa présence peut être vérifiée avec :

```bash
php -m | grep pdo_mysql
```

Résultat attendu :

```text
pdo_mysql
```

---

# Composer

Composer est installé dans le conteneur `app`.

Sa présence peut être vérifiée avec :

```bash
composer --version
```

Composer sera utilisé pour installer Laravel et gérer ses dépendances PHP.

---

# Conteneur `mysql`

Le conteneur `mysql` héberge le serveur de bases de données **MySQL 8.4**.

Le nom du serveur sur le réseau Docker est :

```text
mysql
```

Le port interne utilisé par MySQL est :

```text
3306
```

Aucune base de données applicative n'est créée automatiquement.

Les bases nécessaires aux projets doivent être créées manuellement.

Les données sont stockées dans le volume Docker :

```text
mysql-data
```

Ce volume assure la persistance des bases de données lors du redémarrage des conteneurs.

---

# Conteneur `phpmyadmin`

Le conteneur `phpmyadmin` fournit une interface Web d'administration du serveur MySQL.

phpMyAdmin est accessible depuis le port :

```text
8080
```

Paramètres de connexion :

```text
Serveur      : mysql
Utilisateur  : root
Mot de passe : root
```

---

# Fichiers de configuration

L'environnement repose sur trois fichiers principaux :

```text
LaravelBase
│
├── .devcontainer
│   ├── devcontainer.json
│   └── Dockerfile
│
├── docker-compose.yml
│
└── projetLaravel
```

---

## `devcontainer.json`

Le fichier :

```text
.devcontainer/devcontainer.json
```

configure GitHub Codespaces et VS Code.

Il définit notamment :

- le fichier `docker-compose.yml` utilisé ;
- le conteneur `app` comme environnement de développement ;
- le dossier `/workspace/projetLaravel` comme dossier de travail ;
- les ports `8000` et `8080` ;
- les extensions VS Code installées automatiquement.

---

## `docker-compose.yml`

Le fichier :

```text
docker-compose.yml
```

définit et orchestre les trois conteneurs :

- `app` ;
- `mysql` ;
- `phpmyadmin`.

Il définit également :

- les ports ;
- les volumes ;
- les paramètres MySQL ;
- les relations entre les conteneurs.

Le port Apache est publié de la manière suivante :

```text
8000:80
```

Le port `8000` du Codespace permet ainsi d'accéder au port `80` d'Apache dans le conteneur `app`.

---

## `Dockerfile`

Le fichier :

```text
.devcontainer/Dockerfile
```

construit le conteneur `app`.

Il utilise comme image de base :

```text
php:8.4-apache
```

Il ajoute notamment :

- Git ;
- unzip ;
- PDO MySQL ;
- le module Apache `rewrite` ;
- Composer.

Apache est également configuré pour utiliser :

```text
/workspace/projetLaravel/public
```

comme dossier Web.

---

# Rôle des fichiers de configuration

```text
devcontainer.json
        │
        │ Configure Codespaces et VS Code
        ▼
docker-compose.yml
        │
        │ Définit et relie les conteneurs
        ▼
Dockerfile
        │
        │ Construit le conteneur app
        ▼
Apache + PHP 8.4 + Composer + PDO MySQL
```

---

# État actuel du dépôt modèle

Les composants suivants sont installés et configurés :

- GitHub Codespaces ;
- Docker Compose ;
- Apache ;
- PHP 8.4 ;
- Composer ;
- PDO MySQL ;
- MySQL 8.4 ;
- phpMyAdmin ;
- extensions VS Code pour le développement PHP et Laravel.

Les composants suivants ont été vérifiés dans un Codespace neuf :

```bash
php -v
composer --version
php -m | grep pdo_mysql
apache2ctl -v
```

> **Laravel n'est pas installé ni paramétré dans le dépôt modèle.**

Le dossier :

```text
/workspace/projetLaravel
```

est destiné à accueillir le futur projet Laravel.

---

# Créer un nouveau projet

`LaravelBase` est configuré comme dépôt modèle GitHub.

Pour créer un nouveau projet :

1. Cliquer sur `Use this template`.
2. Choisir `Create a new repository`.
3. Donner un nom au nouveau dépôt.
4. Créer le dépôt.
5. Créer un Codespace depuis ce nouveau dépôt.

GitHub Codespaces construit automatiquement l'environnement.

Les trois conteneurs sont démarrés :

```text
app
mysql
phpmyadmin
```

Apache démarre automatiquement dans le conteneur `app`.

---

# Installation future de Laravel

Dans le terminal du Codespace, se placer dans :

```text
/workspace/projetLaravel
```

Installer Laravel :

```bash
composer create-project laravel/laravel .
```

Vérifier l'installation :

```bash
php artisan --version
```

Après l'installation, Apache sert automatiquement le dossier :

```text
/workspace/projetLaravel/public
```

L'application Laravel est accessible depuis le port `8000`.

Il n'est pas nécessaire d'exécuter :

```bash
php artisan serve
```

---

# Configuration de MySQL dans Laravel

Après avoir créé une base de données dans phpMyAdmin, modifier le fichier `.env` de Laravel.

Exemple :

```env
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=nom_de_la_base
DB_USERNAME=root
DB_PASSWORD=root
```

Le nom :

```text
mysql
```

correspond au service MySQL défini dans `docker-compose.yml`.

La connexion peut ensuite être testée avec :

```bash
php artisan migrate
```
