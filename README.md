# Projet Demo - Installation Nginx et MySQL sur Ubuntu

Ce projet documente l'installation et la configuration d'une application web "demo" avec Nginx comme serveur web et MySQL comme base de données sur un système Ubuntu.

## Prérequis

- Ubuntu Server (20.04 LTS ou supérieur)
- Un utilisateur avec des droits sudo
- Minimum 2 Go de RAM
- 10 Go d'espace disque disponible
- Connexion Internet stable

## Structure du Projet

```
/var/www/demo/
├── index.html
├── css/
├── js/
└── images/

/etc/nginx/
├── nginx.conf
└── sites-available/
    └── demo

/var/log/nginx/
├── demo.access.log
└── demo.error.log
```

## Installation

### 1. Installation de Nginx

```bash
# Mise à jour du système
sudo apt update
sudo apt upgrade -y

# Installation de Nginx
sudo apt install nginx -y

# Démarrage et activation du service
sudo systemctl start nginx
sudo systemctl enable nginx
```

### 2. Configuration de Nginx

1. Créez le répertoire de l'application :
```bash
sudo mkdir -p /var/www/demo
sudo chown -R $USER:www-data /var/www/demo
```

2. Copiez les fichiers de configuration Nginx :
```bash
sudo cp nginx.conf /etc/nginx/nginx.conf
sudo nginx -t
sudo systemctl restart nginx
```

### 3. Installation de MySQL

```bash
# Installation du serveur MySQL
sudo apt install mysql-server -y

# Sécurisation de l'installation
sudo mysql_secure_installation
```

### 4. Configuration de MySQL

```bash
# Connexion à MySQL
sudo mysql

# Création de la base et de l'utilisateur
mysql> CREATE DATABASE demo;
mysql> CREATE USER 'demo_user'@'localhost' IDENTIFIED BY 'votre_mot_de_passe';
mysql> GRANT ALL PRIVILEGES ON demo.* TO 'demo_user'@'localhost';
mysql> FLUSH PRIVILEGES;
```

## Configuration

### Configuration Nginx

Le fichier de configuration principal (`nginx.conf`) inclut :
- Configuration du serveur web
- Gestion des logs
- Configuration CORS
- Gestion du cache
- Proxy pour les API
- Configuration de sécurité

Points importants de la configuration :
- Port d'écoute : 80
- Nom du serveur : demo.local
- Racine du site : /var/www/demo
- Logs spécifiques dans /var/log/nginx/demo.*.log

### Configuration MySQL

- Port par défaut : 3306
- Base de données : demo
- Utilisateur : demo_user

## Déploiement

1. Déployez vos fichiers d'application :
```bash
cp -r votre_app/* /var/www/demo/
```

2. Ajustez les permissions :
```bash
sudo chown -R www-data:www-data /var/www/demo
sudo chmod -R 755 /var/www/demo
```

3. Vérifiez la configuration :
```bash
sudo nginx -t
```

4. Redémarrez les services :
```bash
sudo systemctl restart nginx
sudo systemctl restart mysql
```

## Test et Vérification

### Test Nginx

1. Vérifiez le statut du service :
```bash
sudo systemctl status nginx
```

2. Testez l'accès HTTP :
```bash
curl http://localhost
```

### Test MySQL

1. Vérifiez le statut du service :
```bash
sudo systemctl status mysql
```

2. Testez la connexion :
```bash
mysql -u demo_user -p
```

## Maintenance

### Logs

- Logs Nginx : `/var/log/nginx/demo.*.log`
- Logs MySQL : `/var/log/mysql/error.log`

### Sauvegarde

1. Sauvegarde de la base de données :
```bash
mysqldump -u demo_user -p demo > backup.sql
```

2. Sauvegarde des fichiers :
```bash
sudo tar -czf demo-files.tar.gz /var/www/demo
```

## Dépannage

### Problèmes Courants Nginx

1. **Erreur 502 Bad Gateway**
   - Vérifiez si le backend est en cours d'exécution
   - Vérifiez les logs Nginx

2. **Erreur 403 Forbidden**
   - Vérifiez les permissions des fichiers
   - Vérifiez la configuration SELinux

### Problèmes Courants MySQL

1. **Impossible de se connecter**
   - Vérifiez le service MySQL
   - Vérifiez les permissions utilisateur

2. **Performance lente**
   - Vérifiez la configuration de la mémoire
   - Analysez les logs slow query

## Sécurité

- Mise à jour régulière du système
- Configuration du pare-feu (UFW)
- Utilisation de HTTPS (certbot)
- Sécurisation des accès MySQL
- Protection contre les attaques courantes

## Contribution

1. Forkez le projet
2. Créez une branche pour votre fonctionnalité
3. Committez vos changements
4. Poussez vers la branche
5. Créez une Pull Request

## Licence

Ce projet est sous licence MIT. Voir le fichier `LICENSE` pour plus de détails.

## Auteurs

- Maimouna Ndiaye
