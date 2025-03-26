# Documentation : Installation et Configuration d'un PC d'Administration sous Windows 10

## 1️⃣ Introduction
Ce document décrit les étapes d'installation et de configuration d'un **PC d'administration** sous Windows 10 pour gérer l'infrastructure réseau et système.

---

## 2️⃣ Prérequis
- Un PC sous **Windows 10** (édition Pro ou Enterprise recommandée)
- Accès administrateur pour l'installation des outils
- Accès au domaine Active Directory (si applicable)

---

## 3️⃣ Installation de RSAT (Remote Server Administration Tools)
### 📌 Présentation de RSAT
RSAT est un ensemble d'outils permettant d'administrer des serveurs Windows à distance depuis un poste client Windows 10.

### 🛠️ Installation de RSAT
1. **Ouvrir Windows PowerShell en mode administrateur**
2. **Exécuter la commande suivante pour lister les outils RSAT disponibles :**
   ```powershell
   Get-WindowsCapability -Name RSAT* -Online
   ```
3. **Installer les outils RSAT nécessaires (exemple : GPMC et ADUC) :**
   ```powershell
   Add-WindowsCapability -Online -Name "RSAT:GroupPolicy.Management.Tools~~~~0.0.1.0"
   Add-WindowsCapability -Online -Name "RSAT:ActiveDirectory-Tools~~~~0.0.1.0"
   ```
4. **Vérifier l'installation :**
   ```powershell
   Get-WindowsCapability -Name RSAT* -Online | Where-Object {$_.State -eq "Installed"}
   ```

![Capture d'écran 2025-03-26 142435](https://github.com/user-attachments/assets/34a1fb5d-529a-4d61-b03b-510bf2e99b42)
![Capture d'écran 2025-03-26 142622](https://github.com/user-attachments/assets/4c353e17-e15f-43d7-9995-e4a4c5531e39)

---

## 4️⃣ Activation du Bureau à Distance (RDP)
### 📌 Objectif
Le Bureau à Distance (RDP) permet d'accéder au PC d'administration à distance tout en sécurisant les connexions.

### 🛠️ Procédure
1. **Ouvrir les paramètres Windows** :
   - Aller dans `Paramètres` > `Système` > `Bureau à distance`
   - Activer **Autoriser les connexions Bureau à distance**
2. **Restreindre l'accès RDP à certains utilisateurs :**
   - Cliquer sur **Utilisateurs autorisés à se connecter à distance**
   - Ajouter uniquement le groupe dédié (`DSI_Admins` par exemple)
3. **Appliquer des règles de sécurité supplémentaires :**
   - Activer l'authentification au niveau du réseau (NLA)
   - Changer le port RDP par défaut dans le registre (`3389` → ex: `12345`)

     ![Capture d'écran 2025-03-26 145410](https://github.com/user-attachments/assets/d1b99c47-2840-4501-a40a-ad98052666e3)

   
---

## 5️⃣ Configuration des GPO pour Restreindre l'Accès
### 📌 Objectif
Créer une **stratégie de groupe (GPO)** pour autoriser uniquement les utilisateurs du groupe **DSI** à accéder au PC d'administration.

### 🛠️ Création de la GPO
1. **Ouvrir la Console de Gestion des Stratégies de Groupe (GPMC)**
   - `Démarrer` → `Exécuter` → taper `gpmc.msc`
2. **Créer une nouvelle GPO**
   - Dans `Forêt > Domaines > [Votre Domaine]`
   - Clic droit sur l’OU où se trouve le PC d’administration → `Créer une GPO`
   - Nommer la GPO : **GPO_Restriction_Access_AdminPC**
3. **Configurer la restriction d'accès**
   - Aller dans :
     ```
     Configuration Ordinateur > Stratégies > Paramètres Windows > Paramètres de sécurité > Stratégies locales > Attribution des droits utilisateur
     ```
   - Modifier **Autoriser l’ouverture de session via Bureau à Distance**
   - Supprimer `Utilisateurs du Bureau à Distance`
   - Ajouter uniquement le groupe `DSI_Admins`
4. **Appliquer et lier la GPO au bon conteneur AD**
   - Clic droit sur l'OU contenant le PC d'administration → `Lier une GPO existante`
   - Sélectionner `GPO_Restriction_Access_AdminPC`
5. **Forcer l’application de la GPO sur le poste client**
   ```powershell
   gpupdate /force
   ```
![Capture d'écran 2025-03-26 153454](https://github.com/user-attachments/assets/28264bbf-e72b-4532-b076-8d0f702a87d9)
![Capture d'écran 2025-03-26 150139](https://github.com/user-attachments/assets/93b3bc76-9f1d-4c82-aa4f-6c575a71fe5a)
![Capture d'écran 2025-03-26 152340](https://github.com/user-attachments/assets/5622dcf9-b195-4bf8-950f-a69b051961f9)
![Capture d'écran 2025-03-26 152602](https://github.com/user-attachments/assets/4c650609-dfd6-4604-b14c-1f6cfeff006a)
![Capture d'écran 2025-03-26 152628](https://github.com/user-attachments/assets/4f1e43ae-dd56-4f16-b436-315761630d18)
![Capture d'écran 2025-03-26 153036](https://github.com/user-attachments/assets/f7b9561f-4c2a-45a9-b591-b97c3ff4d58e)
![Capture d'écran 2025-03-26 153454](https://github.com/user-attachments/assets/f1ea3099-796f-46c0-8b15-1a69ef880543)
![Capture d'écran 2025-03-26 153827](https://github.com/user-attachments/assets/ab543dfa-028c-4d01-baf2-2641b27b117f)

---

## 6️⃣ Tests et Validation
### ✅ Vérifications à effectuer
- Vérifier que **seuls les membres du groupe DSI** peuvent se connecter en RDP.
- Tester l’accès aux outils RSAT depuis le PC d’administration.
- Vérifier les logs d’accès (`eventvwr.msc` → Journaux Windows > Sécurité).

---

## 7️⃣ Conclusion
Le PC d'administration est désormais sécurisé et prêt à être utilisé pour la gestion de l’infrastructure IT. 🎯


