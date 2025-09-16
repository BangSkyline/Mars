# 🚀 Mars

[![GitHub Repo stars](https://img.shields.io/github/stars/BangSkyline/Mars?style=social)](https://github.com/BangSkyline/Mars)
[![Docker Compose](https://img.shields.io/badge/Docker%20Compose-1.29%2B-blue)](https://docs.docker.com/compose/)
[![GLPI](https://img.shields.io/badge/GLPI-10.x-green)](https://glpi-project.org/)

---

## 📖 Description

**Mars** est une configuration Docker Compose prête à l'emploi pour déployer [GLPI](https://glpi-project.org/), une solution open-source de gestion des actifs informatiques, de suivi des incidents et de service desk. Cette configuration simplifie l'installation de GLPI avec ses dépendances essentielles, incluant un serveur web, une base de données et un environnement PHP, le tout conteneurisé pour une gestion et un déploiement facilités.

Que vous souhaitiez mettre en place un nouveau système de gestion d'inventaire informatique ou tester GLPI dans un environnement de développement, Mars vous permet de démarrer rapidement avec un minimum de configuration.

---

## ✨ Fonctionnalités

- **Services préconfigurés** : Inclut l'application GLPI, une base de données MariaDB et un serveur web Nginx.
- **Données persistantes** : Volumes pour la base de données et les fichiers GLPI pour garantir la pérennité des données après redémarrage.
- **Variables d'environnement** : Personnalisation facile via un fichier `.env` pour les identifiants de la base de données, les ports, etc.
- **Prêt pour la production** : Supporte HTTPS, les sauvegardes et la mise à l'échelle si nécessaire.
- **Compatible avec GLPI 10.x** : Utilise des images Docker officielles lorsque possible.

---

## 🛠 Prérequis

Avant de commencer, assurez-vous d'avoir installé :

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
   - `HTTP_PORT` : Port pour accéder à GLPI (par défaut : 8080).

3. **Démarrer les services** :
   Lancez la commande suivante pour télécharger les images et démarrer les conteneurs en mode détaché :
   ```bash
   docker compose up -d
   ```

4. **Initialiser GLPI** :
   - Attendez que la base de données soit initialisée (vérifiez les logs avec `docker compose logs db`).
   - Accédez à GLPI via `http://localhost:8080` (ou le port configuré).
   - Suivez l'assistant d'installation web de GLPI :
     - Hôte de la base de données : `db` (nom interne du conteneur).
     - Nom de la base de données : `glpi`.
     - Utilisateur/Mot de passe : Comme défini dans `.env`.
   - Après l'installation, connectez-vous avec les identifiants par défaut (admin / glpi) et changez immédiatement le mot de passe.

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

- **Accéder à GLPI** : Ouvrez votre navigateur et allez à `http://localhost:8080` (remplacez par votre hôte/IP et port).
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
| glpi    | glpi/glpi:10-apache | Application GLPI avec Apache | 8080:80 |
| db      | mariadb:10.11 | Base de données MariaDB | 3306 (interne) |
| nginx   | nginx:alpine (optionnel) | Serveur web/proxy inverse | 80:80, 443:443 |

---

## 🛠 Dépannage

- **Problèmes de connexion à la base de données** : Assurez-vous que le service `db` est sain (`docker compose logs db`). Redémarrez si nécessaire.
- **Erreurs de permissions** : Exécutez `docker compose exec glpi chown -R www-data:www-data /var/www/html` dans le conteneur GLPI.
- **Conflits de ports** : Modifiez `HTTP_PORT` dans `.env` si 8080 est déjà utilisé.
- **Logs** : Vérifiez toujours `docker compose logs <service>` pour les erreurs.
- **Documentation GLPI** : Consultez la [documentation officielle de GLPI](https://glpi-project.org/documentation/) pour les problèmes spécifiques à l'application.

Si vous rencontrez des problèmes non couverts ici, ouvrez une issue sur le [dépôt GitHub](https://github.com/BangSkyline/Mars/issues).

---

## 🤝 Contribution

Les contributions sont les bienvenues ! Veuillez suivre ces étapes :

1. Forkez le dépôt.
2. Créez une branche pour votre fonctionnalité (`git checkout -b feature/NouvelleFonctionnalité`).
3. Validez vos modifications (`git commit -m 'Ajout de NouvelleFonctionnalité'`).
4. Poussez vers la branche (`git push origin feature/NouvelleFonctionnalité`).
5. Ouvrez une Pull Request.

Pour des modifications majeures, veuillez d'abord ouvrir une issue pour en discuter.

---

## 📜 Licence

Ce projet est sous licence MIT - voir le fichier [LICENSE](LICENSE) pour plus de détails (à ajouter si absent).

---

## 🙏 Remerciements

- [Projet GLPI](https://glpi-project.org/) pour son incroyable logiciel open-source.
- Équipes Docker et Docker Compose pour les outils de conteneurisation.
- Images Docker officielles des communautés GLPI et MariaDB.

---

*Construit avec ❤️ pour des déploiements GLPI simplifiés. Si ce projet vous est utile, mettez une étoile au dépôt !*