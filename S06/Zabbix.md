# 🖥️ Installation de Zabbix 7.2 sur Debian 12

## Prérequis
- Une machine Debian 12 à jour
- Accès root ou un utilisateur avec privilèges sudo
- Serveur web (NGINX ou Apache) installé sur Debian
- Base de données (MariaDB ou PostgreSQL) également installé

## Étape 1 : Mettre à jour le système
```bash
sudo apt update && sudo apt upgrade -y
```

## Étape 2 : Installer les paquets nécessaires
```bash
sudo apt install -y gnupg2 curl software-properties-common
```

## Étape 3 : Ajouter le dépôt Zabbix
```bash
curl -fsSL https://repo.zabbix.com/zabbix/7.2/debian/pool/main/z/zabbix-release/zabbix-release_7.2-1%2Bdebian12_all.deb -o zabbix-release.deb
sudo dpkg -i zabbix-release.deb
sudo apt update
```

## Étape 4 : Installer Zabbix Server, Web et Agent
```bash
sudo apt install -y zabbix-server-mysql zabbix-frontend-php zabbix-nginx-conf zabbix-agent
```

## Étape 5 : Installer et configurer MariaDB
```bash
sudo apt install -y mariadb-server
sudo systemctl enable --now mariadb
```

### Configurer la base de données Zabbix
```bash
sudo mysql -uroot -p
```
configuration dans la console MariaDB :
```sql
CREATE DATABASE zabbix CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;
CREATE USER 'zabbix'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON zabbix.* TO 'zabbix'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```
Note : il faut remplacer 'zabbix' par le nom que vous aurez choisi ainsi que 'password' avec le mot de passe choisi

Importer le schéma de base de données :
```bash
zcat /usr/share/doc/zabbix-server-mysql/create.sql.gz | mysql -uzabbix -p zabbix
```

## Étape 6 : Configurer Zabbix Server dans le fichier zabbix_server.conf
```bash
sudo nano /etc/zabbix/zabbix_server.conf
```
Modifier les lignes suivantes en rentrant vos users et vos mots de passe :
```
DBName=zabbix
DBUser=zabbix
DBPassword=password
```
Note : pareil que pour la console MariaDB, il faut remplacer 'zabbix' par le nom que vous aurez choisi ainsi que 'password' avec le mot de passe choisi

## Étape 7 : Configurer NGINX pour Zabbix dans le fichier nginx.conf
```bash
sudo nano /etc/zabbix/nginx.conf
```
Modifier :
```
listen 80;
server_name your_domain_or_ip;
```
Note : Il faudra rentrer votre nom de serveur ou votre adresse IP

Redémarrer les services :
```bash
sudo systemctl restart zabbix-server zabbix-agent nginx php8.2-fpm
```
Activer au démarrage :
```bash
sudo systemctl enable zabbix-server zabbix-agent nginx php8.2-fpm
```

## Étape 8 : Accéder à l'interface web
Ouvrir un navigateur et accéder à :
```
http://your_domain_or_ip
```

Suivre l'assistant d'installation en fournissant les informations de la base de données.

## Étape 9 : Finaliser l'installation
Connectez-vous avec :
- **Utilisateur** : `Admin`
- **Mot de passe** : `zabbix`

Note : Il faudra bien sur remplacer 'Admin' par le nom que vous aurez choisi ainsi que 'zabbix' avec le mot de passe choisi

Zabbix est maintenant installé et prêt à être utilisé ! 🎉

