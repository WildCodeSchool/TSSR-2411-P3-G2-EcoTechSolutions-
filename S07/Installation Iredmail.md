# Documentation Complète : Installation et Configuration de iRedMail sur Debian 12 avec intégration à Active Directory Windows Server 2022


---

## ✅ Prérequis

### Matériel et logiciel
- Une machine Debian 12 fraîchement installée.
- Un serveur Windows Server 2022 avec Active Directory configuré.
- Un accès administrateur sur les deux machines.
- Un nom de domaine valide (ex: `mail.EcoTechSolutions.com`).
- Avoir une adresse Ip statique sur son serveur Debian ainsi que la même adresse IP DNS pour Windows Server et pour le serveur Debian

### Mise à jour de Debian
```bash
sudo apt update && sudo apt upgrade -y
```

---

## 🚀 Installation de iRedMail

### Étape 1 : Télécharger le script d'installation

```bash
cd /tmp
wget https://github.com/iredmail/iRedMail/archive/refs/tags/1.7.2.tar.gz
```

*Il est très important de bien vérifier la dernière version sur le site officiel de iRedMail. Ici nous avons utiliser la version 1.7.2*

### Étape 2 : Extraire et lancer le script

```bash
tar -xvzf 1.7.2.tar.gz
cd iRedMail-1.7.2/
sudo bash iRedMail.sh
```

### Étape 3 : Suivre l'installation interactive

Lors de l'installation, vous serez invité à :
- **Choisir le répertoire d'installation** (par défaut : `/var/vmail`)
- **Choisir le serveur web** (Nginx recommandé)
- **Choisir le backend de stockage** (choisissez **LDAP** si vous prévoyez de vous connecter à Active Directory)
- **Définir le mot de passe de l'administrateur iRedMail**
- cocher toutes les options pour votre serveur iredmail avant de finir l'installation

L'installation va prendre quelques minutes.

![Capture d'écran 2025-03-06 114142](https://github.com/user-attachments/assets/7f757805-ba93-4a9d-b952-8d5c104c22b4)
![Capture d'écran 2025-03-06 114508](https://github.com/user-attachments/assets/1b5dc023-30e7-4a5b-b025-2925a204a16f)
![image](https://github.com/user-attachments/assets/dfcb9d06-0b9a-4e8c-b0e5-0e5db5785179)



### Étape 4 : Finalisation

À la fin, les identifiants et l'URL pour accéder à l'interface d'administration vous seront fournis (ex: `https://mail.mondomaine.com/iredadmin`).

---



