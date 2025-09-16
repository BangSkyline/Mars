# 🚀 Mars

---

## 📖 Description

Docker Compose prête à l'emploi pour déployer [GLPI](https://glpi-project.org/), une solution open-source de gestion des actifs informatiques, de suivi des incidents et de service desk. Cette configuration simplifie l'installation de GLPI avec ses dépendances essentielles, incluant un serveur web, une base de données et un environnement PHP, le tout conteneurisé pour une gestion et un déploiement facilités.

---

## ✨ Fonctionnalités

- **Services préconfigurés** : Inclut l'application GLPI & une base de données MariaDB.
- **Données persistantes** : Volumes pour la base de données et les fichiers GLPI pour garantir la pérennité des données après redémarrage.
- **Variables d'environnement** : Personnalisation facile via un fichier `.env` pour les identifiants de la base de données, les ports, etc.
- **Prêt pour la production** : Supporte HTTPS, les sauvegardes et la mise à l'échelle si nécessaire.
- **Compatible avec GLPI 10.x** : Utilise des images Docker officielles lorsque possible.

---

## 🛠 Prérequis

- [Docker](https://docs.docker.com/get-docker/) (version 20.10+)
- [Docker Compose](https://docs.docker.com/compose/install/) (version 1.29+ ou Docker Compose V2)
- Des connaissances de base sur Docker et les opérations en ligne de commande

---

## 📦 Installation

1. **Cloner le dépôt** :
   ```bash
   git clone https://github.com/BangSkyline/Mars.git
   cd Mars
   ```

2. **Copier le fichier d'environnement** :
   Copiez le fichier d'exemple et personnalisez-le selon vos besoins (par exemple, modifiez les mots de passe ou les ports) :
   ```bash
   cp .env.example .env
   ```

   Modifiez `.env` avec vos valeurs préférées :
   - `DB_ROOT_PASSWORD` : Mot de passe root pour MariaDB.
   - `DB_USER` et `DB_PASSWORD` : Identifiants pour l'utilisateur de la base de données GLPI.
   - `GLPI_ADMIN_PASSWORD` : Mot de passe initial pour l'administrateur GLPI (à changer après la première connexion !).

3. **Démarrer les services** :
   Lancez la commande suivante pour télécharger les images et démarrer les conteneurs en mode détaché :
   ```bash
   docker compose up -d
   ```

4. **Initialiser GLPI** :
   - Attendez que la base de données soit initialisée (vérifiez les logs avec `docker compose logs db`).
   - Accédez à GLPI via `http://localhost:82` (ou le port configuré).
   - Suivez l'assistant d'installation web de GLPI :
     - Hôte de la base de données : `glpi-db` (nom interne du conteneur).
     - Utilisateur/Mot de passe : Comme défini dans `.env`.
   - Après l'installation, connectez-vous avec les identifiants par défaut (admin / glpi) et changez le mot de passe.

5. **Vérifier la configuration** :
   Vérifiez l'état de tous les services :
   ```bash
   docker compose ps
   ```
   Consultez les logs pour dépanner :
   ```bash
   docker compose logs -f
   ```

---

## 🚀 Utilisation

- **Accéder à GLPI** : Ouvrez votre navigateur et allez à `http://localhost:82` (remplacez par votre hôte/IP et port).
- **Connexion administrateur** : Utilisez `glpi` comme nom d'utilisateur et le mot de passe défini dans `GLPI_ADMIN_PASSWORD` (ou par défaut si non personnalisé).
- **Arrêter les services** : `docker compose down`
- **Mise à jour** : Téléchargez les dernières images avec `docker compose pull` et redémarrez avec `docker compose up -d`.

### ⚙️ Configuration personnalisée

- **Support HTTPS** : Pour activer SSL, ajoutez un reverse proxy comme Traefik ou configurez Nginx avec des certificats. Montez vos certificats dans le volume `nginx`.
- **Sauvegarde** : Utilisez `docker compose exec db mysqldump -u root -p glpi > backup.sql` pour les sauvegardes de la base de données. Planifiez via cron ou des outils comme Duplicati.
- **Mise à l'échelle** : Pour la production, envisagez d'ajouter des réplicas ou d'utiliser Docker Swarm.

---

## 📂 Structure du projet

```
Mars/
├── docker-compose.yml      # Fichier Compose principal définissant les services
├── .env.example            # Exemple de variables d'environnement
├── nginx/                  # Configuration Nginx (surcharges optionnelles)
│   └── default.conf
├── glpi/                   # Volume pour les fichiers et configurations GLPI
└── README.md               # Ce fichier
```

---

## 🔍 Aperçu des services

| Service | Image | Rôle | Ports |
|---------|-------|------|-------|
| glpi    | glpi/glpi:10-apache | Application GLPI avec Apache | 82:80 |
| db      | mariadb:10.11 | Base de données MariaDB | 3306 (interne) |

---

[![GitHub Repo stars](https://img.shields.io/github/stars/BangSkyline/Mars?style=social)](https://github.com/BangSkyline/Mars)
[![Docker Compose](https://img.shields.io/badge/Docker%20Compose-1.29%2B-blue)](https://docs.docker.com/compose/)
[![GLPI](https://img.shields.io/badge/GLPI-10.x-green)](https://glpi-project.org/)

*Si ce projet vous est utile, mettez une étoile au dépôt !*
