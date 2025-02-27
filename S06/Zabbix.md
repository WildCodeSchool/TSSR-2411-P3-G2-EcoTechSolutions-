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
![image](https://github.com/user-attachments/assets/4a633d82-45ff-47b1-8bee-2dd36a080ee2)
![image](https://github.com/user-attachments/assets/dc3a95b1-4574-4e6c-973d-7ab864f73644)



## Étape 4 : Installer Zabbix Server, Web et Agent
```bash
sudo apt install -y zabbix-server-mysql zabbix-frontend-php zabbix-nginx-conf zabbix-agent
```

![image](https://github.com/user-attachments/assets/0bedf5dd-1f63-49f1-a4b7-4d8b580db908)


## Étape 5 : Installer et configurer MariaDB
```bash
sudo apt install -y mariadb-server
sudo systemctl enable --now mariadb
```
![image](https://github.com/user-attachments/assets/3f38f2a2-1aa2-47b7-9019-30c71c872000)
![image](https://github.com/user-attachments/assets/c5d26d68-f768-456e-82b8-f39e74f000f5)



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

![image](https://github.com/user-attachments/assets/2fbfa782-0ac7-48df-9c1b-8a7d447aab8d)
![image](https://github.com/user-attachments/assets/21c0f468-6746-403a-b962-6d6f00f8df3b)


Note : il faut remplacer 'zabbix' par le nom que vous aurez choisi ainsi que 'password' avec le mot de passe choisi

Importer le schéma de base de données :
```bash
zcat /usr/share/doc/zabbix-server-mysql/create.sql.gz | mysql -uzabbix -p zabbix
```
![image](https://github.com/user-attachments/assets/573ea28f-4147-4b23-bb5a-18aec147a48b)

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

![image](https://github.com/user-attachments/assets/8094922a-85ae-4626-a7cd-d141d0ba7287)

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

![image](https://github.com/user-attachments/assets/a791a8d9-681f-4659-a394-03f63ebfad56)

Activer au démarrage :
```bash
sudo systemctl enable zabbix-server zabbix-agent nginx php8.2-fpm
```

## Étape 8 : Accéder à l'interface web
Ouvrir un navigateur et accéder à :
```
http://your_domain_or_ip
```
![image](https://github.com/user-attachments/assets/16db91fb-cdb7-4b8f-b6ce-6db818e34311)

Suivre l'assistant d'installation en fournissant les informations de la base de données.

## Étape 9 : Finaliser l'installation
Connectez-vous avec :
- **Utilisateur** : `Admin`
- **Mot de passe** : `zabbix`

![image](https://github.com/user-attachments/assets/fc2d7879-ee69-4ac1-b200-cf4d3b1b9c1d)


Note : Il faudra bien sur remplacer 'Admin' par le nom que vous aurez choisi ainsi que 'zabbix' avec le mot de passe choisi

Zabbix est maintenant installé et prêt à être utilisé ! 🎉

