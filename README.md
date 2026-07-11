# LaravelBase

## Présentation

Ce dépôt constitue un **environnement de développement prêt à accueillir des projets Laravel**.

Il utilise **GitHub Codespaces** et Docker afin de disposer d'un environnement de développement complet dans le cloud.

Aucune installation locale de PHP, Composer ou MySQL n'est nécessaire.

> **Laravel n'est pas installé ni paramétré dans ce dépôt modèle.**

Chaque dépôt créé à partir de `LaravelBase` pourra installer et configurer son propre projet Laravel.

---

# Architecture de l'environnement

Le Codespace utilise trois conteneurs Docker :

```text
GitHub Codespace
│
├── app
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

Les conteneurs communiquent entre eux grâce au réseau Docker interne.

---

# Conteneur `app`

Le conteneur `app` constitue l'environnement principal de développement.

VS Code est connecté à ce conteneur.

Il contient notamment :

- PHP 8.4 ;
- Composer ;
- Git ;
- un terminal Linux ;
- le pilote PHP `pdo_mysql`.

Le dossier de travail est :

```text
/workspace/projetLaravel
```

Le port `8000` est réservé au serveur de développement Laravel.

---

# Pilote PDO MySQL

Le pilote PHP :

```text
pdo_mysql
```

est installé automatiquement dans le conteneur `app`.

Il permet à PHP et Laravel de communiquer avec le serveur MySQL.

Sa présence peut être vérifiée avec la commande :

```bash
php -m | grep pdo_mysql
```

Résultat attendu :

```text
pdo_mysql
```

---

# Conteneur `mysql`

Le conteneur `mysql` héberge le serveur de bases de données **MySQL 8.4**.

Le nom du serveur sur le réseau Docker est :

```text
mysql
```

Le port MySQL interne est :

```text
3306
```

Aucune base de données applicative n'est créée automatiquement.

Les bases nécessaires aux projets doivent être créées manuellement.

Les données MySQL sont stockées dans le volume Docker :

```text
mysql-data
```

Ce volume assure la persistance des bases de données lors du redémarrage des conteneurs.

---

# Conteneur `phpmyadmin`

Le conteneur `phpmyadmin` fournit une interface Web permettant d'administrer le serveur MySQL.

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

L'environnement utilise trois fichiers principaux :

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

## `devcontainer.json`

Le fichier :

```text
.devcontainer/devcontainer.json
```

configure GitHub Codespaces et VS Code.

Il définit notamment :

- le fichier Docker Compose utilisé ;
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

définit les conteneurs utilisés par l'environnement :

- `app` ;
- `mysql` ;
- `phpmyadmin`.

Il définit également :

- les volumes ;
- les ports ;
- les paramètres MySQL ;
- les relations entre les conteneurs.

---

## `Dockerfile`

Le fichier :

```text
.devcontainer/Dockerfile
```

personnalise le conteneur `app`.

Il utilise l'image PHP 8.4 pour Dev Containers et installe le pilote :

```text
pdo_mysql
```

L'installation est réalisée lors de la construction du conteneur.

Ainsi, chaque nouveau Codespace créé dispose automatiquement du pilote MySQL pour PHP.

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
        │ Personnalise le conteneur app
        ▼
PHP 8.4 + Composer + PDO MySQL
```

---

# État actuel du dépôt modèle

Les composants suivants sont installés et configurés :

- GitHub Codespaces ;
- Docker Compose ;
- PHP 8.4 ;
- Composer ;
- PDO MySQL ;
- MySQL 8.4 ;
- phpMyAdmin ;
- extensions VS Code pour le développement PHP et Laravel.

> **Laravel n'est pas installé ni paramétré dans le dépôt modèle.**

Le dossier :

```text
/workspace/projetLaravel
```

est destiné à accueillir le futur projet Laravel.

---

# Créer un nouveau projet

`LaravelBase` est configuré comme dépôt modèle GitHub.

Pour créer un projet :

1. Cliquer sur `Use this template`.
2. Choisir `Create a new repository`.
3. Donner un nom au nouveau dépôt.
4. Créer le dépôt.
5. Créer un Codespace depuis ce nouveau dépôt.

GitHub Codespaces construit alors automatiquement l'environnement de développement.

Les trois conteneurs sont démarrés :

```text
app
mysql
phpmyadmin
```

Le conteneur `app` contient automatiquement PHP, Composer et PDO MySQL.

---

# Installation de Laravel

Dans le terminal du Codespace :

```bash
cd /workspace/projetLaravel
```

Installer Laravel :

```bash
composer create-project laravel/laravel .
```

Vérifier l'installation :

```bash
php artisan --version
```

---

# Configuration de MySQL dans Laravel

Après avoir créé la base de données dans phpMyAdmin, modifier le fichier `.env` de Laravel.

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

correspond au nom du service MySQL défini dans `docker-compose.yml`.

La connexion peut ensuite être testée avec :

```bash
php artisan migrate
```
