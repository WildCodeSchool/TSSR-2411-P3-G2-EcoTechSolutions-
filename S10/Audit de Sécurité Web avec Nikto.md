
# 🔍 Audit de Sécurité Web avec Nikto

## 🎯 Objectif
Réaliser un **audit de vulnérabilités** d’un serveur web grâce à l’outil **Nikto**, puis appliquer les **corrections recommandées** en fonction des résultats.

---

## 1️⃣ Installation de Nikto sur Debian

### 1. Installer les dépendances nécessaires
```bash
sudo apt update
sudo apt install git perl libnet-ssleay-perl openssl liburi-perl libwww-perl -y
```

### 2. Cloner Nikto depuis GitHub
```bash
git clone https://github.com/sullo/nikto.git
cd nikto/program
```

### 3. Lancer Nikto sur le serveur cible
```bash
perl nikto.pl -h http://10.10.8.57
```

### 4. (Facultatif) Créer un lien symbolique
```bash
sudo ln -s ~/nikto/program/nikto.pl /usr/local/bin/nikto
```

---

## 2️⃣ Scanner de vulnérabilités

Lancez une analyse complète sur le serveur web cible :

```bash
nikto -h http://10.10.8.57
```

📌 Vous pouvez ajouter des options supplémentaires pour un scan plus poussé :

```bash
nikto -h http://10.10.8.57 -output ~/rapport_nikto.html -Format html
```

### 🔎 Exemple de résultats possibles :
- Fichiers ou répertoires accessibles (`/admin`, `/config.php`, etc.)
- Failles potentielles (XSS, injection, version obsolète d'Apache/PHP)
- Certificats SSL mal configurés
- Headers de sécurité manquants (`X-Frame-Options`, `Content-Security-Policy`, etc.)

---

## 3️⃣ Interprétation et correction des failles

### 🔐 Exemples de failles & solutions :

| 🔧 Problème détecté                            | ✅ Correctif recommandé                                              |
|-----------------------------------------------|----------------------------------------------------------------------|
| Fichiers sensibles accessibles (`/admin`)     | Restreindre l’accès via `.htaccess` ou un pare-feu                   |
| HTTP sans redirection HTTPS                   | Rediriger vers HTTPS (via `.htaccess` ou configuration Apache/Nginx)|
| Headers manquants (`X-Frame-Options`, etc.)   | Ajouter les headers dans la configuration du serveur web            |
| Version serveur exposée (`Apache/2.4.41`)     | Masquer la bannière avec `ServerTokens` et `ServerSignature`        |
| Vulnérabilité à XSS ou injections             | Mettre à jour l’application web et valider les entrées utilisateur  |

---

## 4️⃣ Bonnes pratiques post-audit

- 🔁 **Mettre à jour régulièrement** les services web (Apache/Nginx, PHP, CMS, etc.)
- 🧱 **Configurer un pare-feu** ou **reverse proxy** (ex : **pfSense**, déjà en place)
- 🔑 **Appliquer le principe du moindre privilège**
- 🧪 **Tester après chaque modification** pour s’assurer que le problème est résolu

---

## 5️⃣ Résultats de l’audit Nikto du 28/03/2025

L’audit a été réalisé sur le serveur `10.10.8.57` avec **Nikto v2.5.0**. Voici une synthèse des vulnérabilités détectées et de leur signification.

### ✅ Détails techniques observés :

| 🧪 Élément analysé                        | 🔎 Détail                                                                                 | ✅ Recommandation |
|------------------------------------------|-------------------------------------------------------------------------------------------|------------------|
| Version serveur                          | `Apache/2.4.62 (Debian)`                                                                 | Mettre à jour vers `2.4.63` ou + |
| Répertoire racine `/`                   | Risque d’exposition d’inodes via ETags (CVE-2003-1418)                                   | Désactiver les `ETags` dans la config Apache |
| Méthodes HTTP autorisées                | `HEAD, GET, POST, OPTIONS`                                                               | Restreindre si certaines méthodes ne sont pas utiles |
| Répertoire `/css/`                      | Indexation possible                                                                      | Désactiver l’indexation (`Options -Indexes`) |
| Répertoire `/images/`                   | Indexation possible                                                                      | Idem, désactiver l’indexation |
| 🔒 Headers de sécurité manquants         | `Content-Security-Policy`, `Referrer-Policy`, `Permissions-Policy`, `X-Content-Type-Options`, `Strict-Transport-Security` | Ajouter ces headers dans la configuration Apache/Nginx |

---

### 📌 Exemple de configuration Apache recommandée :

Dans ton fichier Apache (ex. `/etc/apache2/sites-available/000-default.conf`) ou via `.htaccess` :

```apache
# Sécurité des en-têtes HTTP
Header always set Content-Security-Policy "default-src 'self';"
Header always set Referrer-Policy "no-referrer-when-downgrade"
Header always set Permissions-Policy "geolocation=(), microphone=()"
Header always set X-Content-Type-Options "nosniff"
Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains"
```

N’oublie pas d’activer le module headers si ce n’est pas déjà fait :

```bash
sudo a2enmod headers
sudo systemctl reload apache2
```

---

### 📅 Bilan
- ✅ **Scan terminé sans erreur**
- 🔥 **12 éléments de sécurité à corriger**
- 📂 **Répertoires potentiellement exposés à désindexer**
- 🛡️ **Mise à jour d’Apache nécessaire**

---
