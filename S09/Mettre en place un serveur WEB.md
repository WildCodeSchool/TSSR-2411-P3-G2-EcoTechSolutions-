
# 📌 Installation d’un serveur web Apache sur Debian 12.7.1

## 📝 Prérequis
- Une machine sous **Debian 12.7.1**
- Un accès avec un utilisateur ayant les droits `sudo`
- Une connexion Internet active

---

## 🚀 Étape 1 : Mise à jour du système

Avant toute installation, mettez à jour votre système avec :

```bash
sudo apt update && sudo apt upgrade -y
```

- ![Mise à jour du système](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S09/Mettre%20en%20place%20un%20serveur%20WEB/01_Mise_a_jour_systeme.png)


Vérifiez que votre système est bien à jour :

```bash
lsb_release -a
```

---

## 🏗️ Étape 2 : Installation d’Apache

Installez Apache avec la commande :

```bash
sudo apt install apache2 -y
```

- ![Installation d'apache](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S09/Mettre%20en%20place%20un%20serveur%20WEB/02_Install_Apache.png)


Vérifiez que le service est bien démarré :

```bash
sudo systemctl status apache2
```
- ![Etat service apache](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S09/Mettre%20en%20place%20un%20serveur%20WEB/03_Etat_service-apache.png)


Si Apache n'est pas actif, démarrez-le :

```bash
sudo systemctl start apache2
```

Et activez-le au démarrage :

```bash
sudo systemctl enable apache2
```

- ![Activation apache démarrage](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S09/Mettre%20en%20place%20un%20serveur%20WEB/04_Activation_demarrage.png)

---

## 🌐 Étape 3 : Configuration du pare-feu (UFW)

Si **UFW** (Uncomplicated Firewall) est activé sur votre serveur, autorisez Apache :

```bash
sudo ufw allow 'Apache Full'
```

Vérifiez les règles de pare-feu :

```bash
sudo ufw status
```

---

## 🔍 Étape 4 : Vérification de l’installation

Pour vérifier que votre serveur Apache fonctionne, ouvrez un navigateur et entrez l’**IP** de votre serveur dans la barre d’adresse :

```
http://10.10.8.57
```

Si tout est correct, vous devriez voir la page de bienvenue d'Apache.

Vous pouvez aussi utiliser la commande suivante pour récupérer l'IP de votre serveur :

```bash
hostname -I
```

- ![Activation apache démarrage](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S09/Mettre%20en%20place%20un%20serveur%20WEB/05_IP.png)

---

## 📂 Étape 5 : Configuration de l’hôte virtuel (Virtual Host)

### 5.1 Création d’un répertoire pour le site web

```bash
sudo mkdir -p /var/www/echotechsolutions
```

Attribuez les droits à votre utilisateur :

```bash
sudo chown -R $USER:$USER /var/www/echotechsolutions
```

Définissez les bonnes permissions :

```bash
sudo chmod -R 755 /var/www/echotechsolutions
```

### 5.2 Création d’une page d’accueil

Créez un fichier `index.html` :

```bash
sudo nano /var/www/echotechsolutions/index.html
```

- ![Page basique](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S09/Mettre%20en%20place%20un%20serveur%20WEB/06_Page_Apache.png)


### 5.3 Configuration de l’hôte virtuel

Créez un fichier de configuration pour votre site :

```bash
sudo nano /etc/apache2/sites-available/echotechsolutions.conf
```

Ajoutez ce contenu :

- ![VirtualHost](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S09/Mettre%20en%20place%20un%20serveur%20WEB/08_VirtualHost.png)


Activez le site :

```bash
sudo a2ensite echotechsolutions.conf
```

Désactivez le site par défaut (optionnel) :

```bash
sudo a2dissite 000-default.conf
```

Rechargez Apache :

```bash
sudo systemctl reload apache2
```

- ![Activation site default](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S09/Mettre%20en%20place%20un%20serveur%20WEB/09_Activation_Site_default.png)

---

## 📜 Étape 6 : Vérification et redémarrage du serveur

Vérifiez la configuration Apache :

```bash
sudo apachectl configtest
```

S’il n’y a pas d’erreur, redémarrez Apache :

```bash
sudo systemctl restart apache2
```

---

## 🎯 Étape 7 : Test final

- Ouvrez un navigateur et tapez **http://10.10.8.57**
- Vérifiez le bon fonctionnement avec :

```bash
curl -I http://localhost
```

- ![Activation site default](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S09/Mettre%20en%20place%20un%20serveur%20WEB/10_Web.png)

---

