# LaravelBase

## Présentation

Ce dépôt contient un environnement de développement hébergé dans **GitHub Codespaces**.

L'objectif est de disposer d'un environnement prêt à accueillir un projet Laravel sans avoir à installer localement PHP, Composer, MySQL ou un serveur de base de données.

Le Codespace est créé à partir de ce dépôt GitHub et s'exécute dans le cloud.

## Architecture de l'environnement

Le Codespace s'appuie sur plusieurs conteneurs Docker définis dans le fichier `docker-compose.yml`.

### Conteneur `app`

Le conteneur `app` constitue l'environnement principal de développement.

Il contient notamment :

- PHP 8.4 ;
- Composer ;
- Git ;
- un terminal Linux ;
- les outils nécessaires au développement PHP.

VS Code est connecté à ce conteneur.

Le dossier de travail utilisé dans VS Code est :

/workspace/projetLaravel

Le port `8000` est prévu pour permettre l'accès à l'application Laravel depuis un navigateur.

### Conteneur `mysql`

Le conteneur `mysql` héberge le serveur de bases de données MySQL 8.4.

Les autres conteneurs communiquent avec ce serveur grâce au réseau Docker interne.

Le nom d'hôte du serveur MySQL est :

mysql

Le port MySQL utilisé sur le réseau interne est :

3306

Les données MySQL sont enregistrées dans un volume Docker nommé `mysql-data`.

Ce volume permet de conserver les bases de données lors du redémarrage des conteneurs.

### Conteneur `phpmyadmin`

Le conteneur `phpmyadmin` fournit une interface Web permettant d'administrer le serveur MySQL.

phpMyAdmin est accessible depuis le port `8080` du Codespace.

Paramètres de connexion :

- Serveur : `mysql`
- Utilisateur : `root`
- Mot de passe : `root`

## Configuration du Codespace

Le fichier `.devcontainer/devcontainer.json` configure l'environnement GitHub Codespaces.

Il indique notamment :

- le fichier Docker Compose à utiliser ;
- le conteneur `app` comme environnement de développement ;
- le dossier `/workspace/projetLaravel` comme dossier de travail ;
- les ports `8000` et `8080` à rendre accessibles ;
- les extensions VS Code à installer.

Lors de la création du Codespace, la commande suivante est également exécutée :

composer --version

Elle permet de vérifier que Composer est correctement installé dans le conteneur `app`.

## Hébergement

GitHub Codespaces est un service proposé par GitHub.

L'infrastructure utilisée pour exécuter les Codespaces repose sur Microsoft Azure.

Lors de la création du Codespace :

1. GitHub prépare l'environnement de développement dans le cloud.
2. Le dépôt GitHub est cloné dans le Codespace.
3. Le fichier `.devcontainer/devcontainer.json` est lu.
4. Docker Compose démarre les conteneurs `app`, `mysql` et `phpmyadmin`.
5. VS Code est connecté au conteneur `app`.

Le développement peut ainsi être réalisé directement dans le navigateur sans cloner le dépôt sur un ordinateur local.

## État actuel du projet

L'environnement de développement est actuellement opérationnel.

Les éléments suivants sont installés et configurés :

- GitHub Codespaces ;
- Docker Compose ;
- PHP 8.4 ;
- Composer ;
- MySQL 8.4 ;
- phpMyAdmin ;
- les extensions VS Code nécessaires au développement PHP.

> **Laravel n'est pas encore installé ni paramétré.**

Le dossier `projetLaravel` est actuellement destiné à accueillir le futur projet Laravel.

La prochaine étape consistera à installer Laravel, puis à configurer sa connexion au serveur MySQL.

## Prochaine étape : installation de Laravel

Laravel sera installé dans le dossier :

/workspace/projetLaravel

L'installation sera réalisée avec Composer.

La configuration du fichier `.env` permettra ensuite de connecter Laravel au conteneur MySQL en utilisant le nom de service Docker `mysql`.
