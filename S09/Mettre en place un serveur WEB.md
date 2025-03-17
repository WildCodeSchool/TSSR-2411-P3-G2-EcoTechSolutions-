
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

Vérifiez que le service est bien démarré :

```bash
sudo systemctl status apache2
```

Si Apache n'est pas actif, démarrez-le :

```bash
sudo systemctl start apache2
```

Et activez-le au démarrage :

```bash
sudo systemctl enable apache2
```

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
http://votre_ip_serveur
```

Si tout est correct, vous devriez voir la page de bienvenue d'Apache.

Vous pouvez aussi utiliser la commande suivante pour récupérer l'IP de votre serveur :

```bash
hostname -I
```

---

## 📂 Étape 5 : Configuration de l’hôte virtuel (Virtual Host)

### 5.1 Création d’un répertoire pour le site web

```bash
sudo mkdir -p /var/www/monsite
```

Attribuez les droits à votre utilisateur :

```bash
sudo chown -R $USER:$USER /var/www/monsite
```

Définissez les bonnes permissions :

```bash
sudo chmod -R 755 /var/www/monsite
```

### 5.2 Création d’une page d’accueil

Créez un fichier `index.html` :

```bash
sudo nano /var/www/monsite/index.html
```

Ajoutez ce contenu :

```html
<!DOCTYPE html>
<html>
<head>
    <title>Bienvenue sur mon serveur Apache</title>
</head>
<body>
    <h1>Apache est bien installé sur Debian 12.7.1 ! 🎉</h1>
</body>
</html>
```

Enregistrez avec `CTRL + X`, puis `Y`, et `ENTRÉE`.

### 5.3 Configuration de l’hôte virtuel

Créez un fichier de configuration pour votre site :

```bash
sudo nano /etc/apache2/sites-available/monsite.conf
```

Ajoutez ce contenu :

```apache
<VirtualHost *:80>
    ServerAdmin admin@monsite.com
    DocumentRoot /var/www/monsite
    ServerName monsite.com
    ServerAlias www.monsite.com
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Activez le site :

```bash
sudo a2ensite monsite.conf
```

Désactivez le site par défaut (optionnel) :

```bash
sudo a2dissite 000-default.conf
```

Rechargez Apache :

```bash
sudo systemctl reload apache2
```

---

## 🔒 Étape 6 : Activation du HTTPS avec Certbot (SSL)

Installez Certbot et le module Apache :

```bash
sudo apt install certbot python3-certbot-apache -y
```

Générez un certificat SSL gratuit avec Let’s Encrypt :

```bash
sudo certbot --apache
```

Suivez les instructions et choisissez l’option **"Rediriger tout le trafic HTTP vers HTTPS"**.

Testez le renouvellement automatique du certificat :

```bash
sudo certbot renew --dry-run
```

---

## 📜 Étape 7 : Vérification et redémarrage du serveur

Vérifiez la configuration Apache :

```bash
sudo apachectl configtest
```

S’il n’y a pas d’erreur, redémarrez Apache :

```bash
sudo systemctl restart apache2
```

---

## 🎯 Étape 8 : Test final

- Ouvrez un navigateur et tapez **http://monsite.com**
- Si vous avez activé SSL, testez aussi **https://monsite.com**
- Vérifiez le bon fonctionnement avec :

```bash
curl -I http://localhost
```

---

## 🎉 Conclusion

Vous avez maintenant un **serveur web Apache** fonctionnel sur **Debian 12.7.1** ! 🚀  
Si vous souhaitez héberger plusieurs sites, vous pouvez **répéter l'étape 5** pour chaque domaine.

---
