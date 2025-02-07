## USER GUIDE CREATION DES GPO STANDARD ET DES GPO DE SECURITE

### GPO de sécurité :
1. [📝 Politique de mot de passe (complexité, longueur, etc.)](#mdp)
2. [🔒Verrouillage de compte (blocage de l'accès à la session après quelques erreur de mot de passe)](#verrouillage)
3. [⛔️ Restriction d'installation de logiciel pour les utilisateurs non-administrateurs](#logiciel)
4. [💽 Restriction des périphériques amovible](#peripherique)
5. [💻 Écran de veille avec mot de passe en sortie](#veille)

### GPO Standart :
1. GPO pour un même fond d'écran
2. Gestion de l'alimentation

---

## Les GPO de sécurité :

1- LA GPO pour la politique de mot de passe :
<span id="mdp"></span> 

# 🔑 Création d'une GPO pour appliquer une politique de mot de passe

## 📝 Objectif
Mettre en place une **stratégie de mot de passe** stricte dans un environnement Active Directory pour renforcer la sécurité des comptes utilisateurs.

---

## 1️⃣ Ouvrir la console de gestion des stratégies de groupe  
1. Connectez-vous à votre **contrôleur de domaine** avec un compte **administrateur du domaine**.
2. Ouvrez **Gestion des stratégies de groupe** :  
   - `Win + R` → tapez `gpmc.msc` → `Entrée`.

---

## 2️⃣ Créer une nouvelle GPO
1. Dans **Gestion des stratégies de groupe**, faites un **clic droit** sur le domaine (`votre-domaine.local`).
2. Cliquez sur **Créer un objet GPO dans ce domaine et le lier ici…**.
3. Nommez la GPO : **Politique de Mot de Passe Sécurisée**.
4. Cliquez sur **OK**.

---

## 3️⃣ Configurer la stratégie de mot de passe
1. Faites un **clic droit** sur la GPO **Politique de Mot de Passe Sécurisée** et cliquez sur **Modifier**.
2. Allez dans :  Configuration ordinateur → Stratégies → Paramètres Windows → Paramètres de sécurité → Stratégies de compte → Politique de mot de passe
3. **Configurer les paramètres** :  
- **Exiger un mot de passe complexe** : `Activé`
  - Exige l’utilisation de **majuscules, minuscules, chiffres et caractères spéciaux**.
- **Longueur minimale du mot de passe** : `12` (ou plus selon votre politique interne).
- **Durée de vie maximale du mot de passe** : `90 jours` (ou selon vos besoins).
- **Durée de vie minimale du mot de passe** : `1 jour` (empêche les changements immédiats pour contourner la politique).
- **Longueur minimale de l’historique du mot de passe** : `5 mots de passe` (évite la réutilisation rapide).
- **Stocker les mots de passe en utilisant un chiffrement réversible** : `Désactivé` (pour éviter que les mots de passe puissent être lus en clair).

4. **Valider les modifications** et fermer l’éditeur de GPO.
 1. **Forcer l’application de la GPO** sur un poste client en exécutant la commande suivante :  gpuptade /force
 2. Vérifier l'application de la stratégie avec la commande : gpresult /r
 3. Tester un changement de mot de passe sur un compte utilisateur (Ctrl + Alt + Suppr → Modifier un mot de passe).
    - Vérifiez que les exigences de complexité sont bien appliquées.
    - Essayez d’utiliser un ancien mot de passe pour voir si l’historique fonctionne.

![image](https://github.com/user-attachments/assets/f1dd3c20-0579-4354-b193-656f9dfc2fc2)


---

## 4️⃣ Appliquer et tester la GPO
1. **Forcer l’application de la GPO** sur un poste client en exécutant la commande suivante :  
```powershell
gpupdate /force


2- La GPO pour le verrouillage de compte :
<span id="verrouillage"></span> 

# 🔒 Création d'une GPO pour bloquer l'installation de logiciels

## 📝 Objectif
Empêcher les utilisateurs non-administrateurs d’installer des logiciels en bloquant **msiexec.exe** via **Software Restriction Policies (SRP)** et configurer **AppLocker** pour un contrôle plus strict.

---

## 1️⃣ Ouvrir la console de gestion des stratégies de groupe  
1. Connectez-vous à votre **contrôleur de domaine** avec un compte **administrateur du domaine**.
2. Ouvrez **Gestion des stratégies de groupe** :  
   - `Win + R` → tapez `gpmc.msc` → `Entrée`.

---

## 2️⃣ Créer une nouvelle GPO
1. Dans **Gestion des stratégies de groupe**, faites un clic droit sur l'OU cible (ex : `utilisateurs` dans `bordeaux`).
2. Cliquez sur **Créer un objet GPO dans ce domaine et le lier ici…**.
3. Nommez la GPO : **Blocage Installation Logiciels**.
4. Cliquez sur **OK**.

---

## 3️⃣ Configurer la restriction avec Software Restriction Policies (SRP)
1. Faites un **clic droit** sur la GPO **Blocage Installation Logiciels** et cliquez sur **Modifier**.
2. Allez dans :  Configuration ordinateur → Stratégies → Paramètres Windows → Paramètres de sécurité → Stratégies de restriction logicielle

![image](https://github.com/user-attachments/assets/fc941fde-00b1-41fc-98bc-e916081cf6c0)

3. Faites un **clic droit** sur **Stratégies de restriction logicielle** → **Créer des stratégies de restriction logicielle**.

### ➤ Ajouter une règle pour bloquer **msiexec.exe**
1. Dans **Stratégies de restriction logicielle**, cliquez sur **Règles supplémentaires**.
2. Faites un **clic droit** dans la partie droite → **Nouvelle règle de chemin**.
3. Dans **Chemin**, entrez : C:\Windows\System32\msiexec.exe
4. Dans **Niveau de sécurité**, sélectionnez **Interdit**.
5. Ajoutez une **description** (ex : *Règle qui bloque directement le processus d'installation d'applications*).
6. Cliquez sur **OK**.

---

## 4️⃣ Configurer AppLocker pour renforcer les restrictions
1. Toujours dans l'éditeur de GPO, allez dans :  
Configuration ordinateur → Stratégies → Paramètres Windows → Paramètres de sécurité → Stratégies de contrôle des applications → AppLocker
2. Cliquez sur **Configurer l’application des règles** et cochez :
- **Executable Rules**
- **Windows Installer Rules**
- **Script Rules**
3. Cliquez sur **OK**.

### ➤ Créer une règle AppLocker pour bloquer `.msi`
1. Allez dans **Windows Installer Rules**.
2. Faites un **clic droit** → **Créer une règle**.
3. Dans l’assistant :
- **Action** : **Refuser**
- **Utilisateur ou groupe** : **Tout le monde**
- **Condition** : **Par chemin**
- **Chemin** :  
  ```
  C:\Windows\System32\msiexec.exe
  ```
4. Cliquez sur **Suivant** et terminez l’assistant.

---

## 5️⃣ Appliquer et tester la GPO
1. **Fermez** l'éditeur de GPO.
2. **Forcer l'application** de la GPO sur un poste client :  gpupdate /force

![image](https://github.com/user-attachments/assets/13c39a56-b2fb-4ac4-85a8-4d2f3a11d07a)

Redémarrez le poste client et tentez d’installer un fichier .msi avec un compte utilisateur standard.


3- La GPO pour restreindre l'installation de logicile pour les utilisateurs qui ne sont pas administrateurs :
<span id="logiciel"></span> 

4- LA GPO des périphériques amovibles :
<span id="periphérique"></span> 

# 📌 Procédure GPO : Restriction des périphériques amovibles

## 🎯 Objectif
Configurer une stratégie de groupe (GPO) pour restreindre l'utilisation des périphériques amovibles (clés USB, disques externes, etc.)

---

## 🛠 Étapes de configuration

### 1️⃣ Ouvrir la console de gestion des stratégies de groupe
1. Se connecter au **contrôleur de domaine**.
2. Ouvrir la console **GPMC** (*Group Policy Management Console*).
   - Appuyer sur `Win + R`, taper `gpmc.msc`, puis valider.

---

### 2️⃣ Créer ou modifier une GPO
1. Naviguer jusqu’à l’OU (*Organizational Unit*) contenant les utilisateurs ou ordinateurs concernés.
2. Clic droit sur l’OU → **Créer une GPO** → Nommer la GPO (ex: `GPO_Restriction_USB`).
3. Clic droit sur la GPO → **Modifier**.

---

### 3️⃣ Configurer la restriction des périphériques amovibles
Dans l’éditeur de stratégie de groupe :

📌 **Chemin :**
```
Configuration ordinateur → Stratégies → Modèles d'administration → Système → Accès au stockage amovible
```

Activer les paramètres suivants :

1. **Refuser l'accès en lecture aux périphériques de stockage amovibles**
   - Double-cliquer sur **"Refuser l'accès en lecture aux périphériques de stockage amovibles"**
   - Sélectionner **Activé**

2. **Refuser l'accès en écriture aux périphériques de stockage amovibles**
   - Double-cliquer sur **"Refuser l'accès en écriture aux périphériques de stockage amovibles"**
   - Sélectionner **Activé**

3. **Refuser l'accès à l'exécution aux périphériques de stockage amovibles**
   - Double-cliquer sur **"Refuser l'accès à l'exécution aux périphériques de stockage amovibles"**
   - Sélectionner **Activé**

4. **Désactiver l'installation de nouveaux périphériques amovibles** (optionnel)
   - Double-cliquer sur **"Tous les classes de stockage amovible : Refuser toutes les classes"**
   - Sélectionner **Activé**

---

### 4️⃣ Appliquer la GPO
1. Fermer l’éditeur et revenir à la **GPMC**.
2. Lier la GPO à l’OU cible :
   - Clic droit → **Lier un objet de stratégie existant...**
3. Forcer l’application de la GPO immédiatement en exécutant sur un poste client :
   ```powershell
   gpupdate /force
   ```

---

## ✅ Vérification sur un poste utilisateur
1. Insérer un périphérique de stockage USB.
2. Vérifier si l’accès est bloqué en lecture et/ou écriture.
3. Tenter d’exécuter un fichier depuis le périphérique pour s’assurer de la restriction.

---

## 🔍 Dépannage
Si la GPO ne s’applique pas immédiatement, redémarrer le poste ou vérifier avec :
```powershell
gpresult /r
```

💡 **Remarque :**
- Pour des restrictions spécifiques à certains utilisateurs, appliquez la GPO sous `Configuration utilisateur` au lieu de `Configuration ordinateur`.
- Tester les paramètres sur un poste avant un déploiement global.

📌 **Fin de la procédure.** 😊


5- LA GPO pour configurer un mot de passe en sortie d'ecran de veille :
<span id="veille"></span> 
## 🎯 Objectif
Configurer une stratégie de groupe (GPO) pour imposer un écran de veille avec une demande de mot de passe à la sortie.

---

## 🛠 Étapes de configuration

### 1️⃣ Ouvrir la console de gestion des stratégies de groupe
1. Se connecter au **contrôleur de domaine**.
2. Ouvrir la console **GPMC** (*Group Policy Management Console*).
   - Appuyer sur `Win + R`, taper `gpmc.msc`, puis valider.

---

### 2️⃣ Créer ou modifier une GPO
1. Naviguer jusqu’à l’OU (*Organizational Unit*) contenant les utilisateurs concernés.
2. Clic droit sur l’OU → **Créer une GPO** → Nommer la GPO (ex: `GPO_Ecran_De_Veille`).
3. Clic droit sur la GPO → **Modifier**.

---

### 3️⃣ Configurer l’écran de veille obligatoire
Dans l’éditeur de stratégie de groupe :

📌 **Chemin :**
```
Configuration utilisateur → Stratégies → Modèles d'administration → Panneau de configuration → Personnalisation
```

Activer les paramètres suivants :

1. **Activer l'économiseur d'écran**
   - Double-cliquer sur **"Activer l'économiseur d'écran"**
   - Sélectionner **Activé**
<br><p align="center"><img src="https://github.com/user-attachments/assets/aa14c715-d08c-4f06-a5f5-814c91bd0f1c" alt=""></p><br>


2. **Forcer l'économiseur d'écran spécifique**
   - Double-cliquer sur **"Forcer l’économiseur d’écran"**
   - Sélectionner **Activé**
   - Renseigner un fichier d’écran de veille, par exemple :
     - `C:\Windows\System32\scrnsave.scr` (écran noir)
     - `C:\Windows\System32\logon.scr` (écran de veille Windows)

3. **Temps d’inactivité avant activation**
   - Double-cliquer sur **"Définir le délai d'inactivité de l'économiseur d'écran"**
   - Sélectionner **Activé**
   - Définir un délai (ex : `900` secondes = 15 minutes).

<br><p align="center"><img src="https://github.com/user-attachments/assets/b7f19797-cd3b-48b8-84b9-9842fbca33f0" alt=""></p><br>


4. **Exiger un mot de passe en sortie**
   - Double-cliquer sur **"Exiger un mot de passe pour la reprise de l'économiseur d'écran"**
   - Sélectionner **Activé**.
<br><p align="center"><img src="https://github.com/user-attachments/assets/5dad398d-38ce-49d3-a4bd-006a7ca6f4a9" alt=""></p><br>

---

### 4️⃣ Appliquer la GPO
1. Fermer l’éditeur et revenir à la **GPMC**.
2. Lier la GPO à l’OU cible :
   - Clic droit → **Lier un objet de stratégie existant...**
3. Forcer l’application de la GPO immédiatement en exécutant sur un poste client :
   ```powershell
   gpupdate /force
   ```

---

## ✅ Vérification sur un poste utilisateur
1. Se connecter avec un compte utilisateur.
2. Laisser l’ordinateur inactif pour vérifier l’activation de l’écran de veille.
3. Bouger la souris ou appuyer sur une touche pour voir si la demande de mot de passe apparaît.

---

## 🔍 Dépannage
Si la GPO ne s’applique pas immédiatement, redémarrer le poste ou vérifier avec :
```powershell
gpresult /r
```

💡 **Remarque :** Assurez-vous que la GPO est bien appliquée à l’OU contenant les comptes utilisateurs et non aux machines si la stratégie est configurée sous `Configuration utilisateur`.

📌 **Fin de la procédure.** 😊

