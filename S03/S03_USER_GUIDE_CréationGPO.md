## USER GUIDE CREATION DES GPO STANDARD ET DES GPO DE SECURITE

### GPO de sécurité :
1. [📝 Politique de mot de passe (complexité, longueur, etc.)](#mdp)
2. [🔒Verrouillage de compte (blocage de l'accès à la session après quelques erreur de mot de passe)](#verrouillage)
3. [⛔️ Restriction d'installation de logiciel pour les utilisateurs non-administrateurs](#logiciel)
4. [💽 Restriction des périphériques amovible](#peripherique)
5. [💻 Écran de veille avec mot de passe en sortie](#veille)
### GPO de sécurité :
1. [💻 Configurer un même fond d'écran](#fondecran)
2. [⚡️parametrer la gestion de l'alimentation](#alimentation)

---

### GPO Standart :
1- GPO pour un même fond d'écran :
<span id="fondecran"></span>

# 🖥️ Création d'une GPO pour définir un fond d’écran commun

## 📝 Objectif
Déployer un **fond d’écran unique** sur tous les postes d’un domaine Active Directory, empêchant les utilisateurs de le modifier.

---

## 1️⃣ Préparer l'image du fond d'écran  
1. Choisissez une **image** au format `.jpg` ou `.bmp` pour garantir la compatibilité.
2. Copiez l’image dans un **dossier réseau accessible à tous les utilisateurs**.  
   - Exemple de chemin réseau partagé :  
     ```
     \\SRV-FILES\Partage\wallpaper.jpg
     ```
   - Assurez-vous que les **utilisateurs ont au moins un accès en lecture** au dossier contenant l’image.
<br><p align="center"><img src="https://github.com/user-attachments/assets/1837afd6-b518-4cd0-a5eb-f215c7b1b14b" alt=""></p><br>


---

## 2️⃣ Ouvrir la console de gestion des stratégies de groupe  
1. Connectez-vous à votre **contrôleur de domaine** avec un compte **administrateur du domaine**.
2. Ouvrez **Gestion des stratégies de groupe** :  
   - `Win + R` → tapez `gpmc.msc` → `Entrée`.

---

## 3️⃣ Créer une nouvelle GPO  
1. Dans **Gestion des stratégies de groupe**, faites un **clic droit** sur le domaine (`votre-domaine.local`) ou une **OU spécifique**.
2. Cliquez sur **Créer un objet GPO dans ce domaine et le lier ici…**.
3. Nommez la GPO : **Fond d’écran Entreprise**.
4. Cliquez sur **OK**.

---

## 4️⃣ Configurer la stratégie de fond d’écran  
1. Faites un **clic droit** sur la GPO **Fond d’écran Entreprise** et cliquez sur **Modifier**.
2. Accédez à :  Configuration utilisateur → Stratégies → Modèles d'administration → Bureau → Bureau actif
3. **Configurer la stratégie** :
- Double-cliquez sur **Fond d’écran de bureau**.
- Sélectionnez **Activé**.
- Dans **Nom du papier peint**, entrez le **chemin réseau** de l’image :  
  ```
  \\SRV-FILES\Partage\wallpaper.jpg
  ```
- Dans **Style du papier peint**, choisissez :
  - `Rempli` (pour adapter l’image à l’écran).
  - `Étiré` si vous souhaitez qu’elle couvre toute la surface.
- Cliquez sur **OK**.

---

## 5️⃣ Appliquer et tester la GPO  
1. **Forcer l’application de la GPO** sur un poste client en exécutant la commande suivante :  gpupdate /force
2. Vérifier l'application de la stratégie avec la commande :gpresult /r
3. Déconnectez-vous et reconnectez-vous pour voir si le fond d’écran est bien appliqué.
<br><p align="center"><img src="https://github.com/user-attachments/assets/86088725-4616-427d-9f6e-2b08600a6fef" alt=""></p><br>
<br><p align="center"><img src="https://github.com/user-attachments/assets/55e1efed-3019-449b-9fd5-6bf1250aaac2" alt=""></p><br>


   

2. [Gestion de l'alimentation](#alimentation):
<span id="alimentation"></span>
## 🎯 Objectif
Configurer une stratégie de groupe (GPO) pour gérer les paramètres d’alimentation des postes du domaine.

---

## 🛠 Étapes de configuration

### 1️⃣ Ouvrir la console de gestion des stratégies de groupe
1. Se connecter au **contrôleur de domaine**.
2. Ouvrir la console **GPMC** (*Group Policy Management Console*).
   - Appuyer sur `Win + R`, taper `gpmc.msc`, puis valider.

---

### 2️⃣ Créer ou modifier une GPO
1. Naviguer jusqu’à l’OU (*Organizational Unit*) contenant les ordinateurs concernés.
2. Clic droit sur l’OU → **Créer une GPO** → Nommer la GPO (ex: `GPO_Gestion_Alimentation`).
3. Clic droit sur la GPO → **Modifier**.

---

### 3️⃣ Configurer les paramètres d’alimentation
Dans l’éditeur de stratégie de groupe :

📌 **Chemin :**
```
Configuration ordinateur → Stratégies → Modèles d'administration → Système → Gestion de l'alimentation
```

Activer les paramètres suivants selon vos besoins :

1. **Exiger un mode de gestion de l’alimentation spécifique**
   - Aller dans **"Paramètres supplémentaires d’alimentation"**
   - Sélectionner **Activé** et choisir un mode d’alimentation prédéfini (ex : "Équilibré", "Économie d’énergie").

2. **Configurer la mise en veille automatique**
   - Double-cliquer sur **"Spécifier le délai d’inactivité avant mise en veille"**
   - Sélectionner **Activé**
   - Définir un délai (ex : `1200` secondes = 20 minutes).
![Gestion_Alimentation](https://github.com/user-attachments/assets/390ba642-26bc-4b18-ad29-9ee7e605d4d7)


3. **Désactiver l’extinction automatique de l’écran**
   - Double-cliquer sur **"Désactiver la mise en veille de l’affichage"**
   - Sélectionner **Activé** (ou configurer un délai d’inactivité selon les besoins).

4. **Empêcher les utilisateurs de modifier les paramètres d’alimentation**
   - Double-cliquer sur **"Interdire aux utilisateurs de modifier les paramètres de gestion de l’alimentation"**
   - Sélectionner **Activé**.

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
1. Aller dans **Panneau de configuration → Options d’alimentation**.
2. Vérifier que le mode d’alimentation appliqué correspond à celui défini dans la GPO.
3. Tester la mise en veille et l’extinction automatique de l’écran après le délai configuré.

---

## 🔍 Dépannage
Si la GPO ne s’applique pas immédiatement, redémarrer le poste ou vérifier avec :
```powershell
gpresult /r
```

💡 **Remarque :**
- Assurez-vous que la GPO est bien appliquée aux machines et non aux utilisateurs si elle est configurée sous `Configuration ordinateur`.
- Testez la GPO sur un poste avant un déploiement global.

📌 **Fin de la procédure.** 😊

---

## Les GPO de sécurité :

# 1- LA GPO pour la politique de mot de passe :
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

---

## 4️⃣ Appliquer et tester la GPO
1. **Forcer l’application de la GPO** sur un poste client en exécutant la commande suivante : gpupdate /force
2. Vérifier l'application de la stratégie avec la commande : gpresult /r
3. Tester un changement de mot de passe sur un compte utilisateur (Ctrl + Alt + Suppr → Modifier un mot de passe).
   - Vérifiez que les exigences de complexité sont bien appliquées.
   - Essayez d’utiliser un ancien mot de passe pour voir si l’historique fonctionne.
<br><p align="center"><img src="https://github.com/user-attachments/assets/569731f0-5878-4576-af52-5427beebd8a9" alt=""></p><br>




# 2- La GPO pour le verrouillage de compte :
<span id="verrouillage"></span> 
# 🔒 Création d'une GPO pour le Verrouillage de compte après plusieurs échecs de connexion

## 📝 Objectif
Configurer une **stratégie de verrouillage de compte** afin de **bloquer temporairement** l’accès à une session après plusieurs tentatives de connexion échouées. Cela permet de protéger les comptes contre les attaques par force brute.

---

## 1️⃣ Ouvrir la console de gestion des stratégies de groupe  
1. Connectez-vous à votre **contrôleur de domaine** avec un compte **administrateur du domaine**.
2. Ouvrez **Gestion des stratégies de groupe** :  
   - `Win + R` → tapez `gpmc.msc` → `Entrée`.

---

## 2️⃣ Créer une nouvelle GPO
1. Dans **Gestion des stratégies de groupe**, faites un **clic droit** sur le domaine (`votre-domaine.local`).
2. Cliquez sur **Créer un objet GPO dans ce domaine et le lier ici…**.
3. Nommez la GPO : **Verrouillage de compte**.
4. Cliquez sur **OK**.

---

## 3️⃣ Configurer la stratégie de verrouillage de compte
1. Faites un **clic droit** sur la GPO **Verrouillage de compte** et cliquez sur **Modifier**.
2. Accédez à :  Configuration ordinateur → Stratégies → Paramètres Windows → Paramètres de sécurité → Stratégies de compte → Stratégie de verrouillage du compte
3. **Configurer les paramètres** :  
- **Seuil de verrouillage du compte** : `5`  
  - 🔹 Bloque le compte après **5 tentatives échouées**.
- **Durée du verrouillage du compte** : `15 minutes`  
  - ⏳ Temps pendant lequel le compte reste verrouillé avant d’être déverrouillé automatiquement.
- **Réinitialisation du compteur de verrouillage** : `15 minutes`  
  - ⏱️ Temps avant que le **compteur de tentatives échouées** soit remis à zéro.

 <br><p align="center"><img src="https://github.com/user-attachments/assets/420d27ba-0267-4a0e-b46f-9e0cd081706e" alt=""></p><br>


4. **Valider les modifications** et fermer l’éditeur de GPO.

---

## 4️⃣ Appliquer et tester la GPO
1. **Forcer l’application de la GPO** sur un poste client en exécutant la commande suivante : gpupdate /force
2. Vérifier l'application de la stratégie avec la commande :gpresult /r
3. Tester la tentative de connexion échouée sur un compte utilisateur :
  - Essayez de taper un mauvais mot de passe plusieurs fois (5 fois dans notre exemple).
  - Vérifiez si le message "Ce compte a été verrouillé" s'affiche après le nombre d'essais défini.
  - Attendez 15 minutes et essayez de vous reconnecte
  - 
<br><p align="center"><img src="https://github.com/user-attachments/assets/7d256993-604c-42e7-b3b8-20277e3afc59" alt=""></p><br>



# 3- La GPO pour restreindre l'installation de logicile pour les utilisateurs qui ne sont pas administrateurs :
<span id="logiciel"></span> 

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

<br><p align="center"><img src="https://github.com/user-attachments/assets/fc941fde-00b1-41fc-98bc-e916081cf6c0" alt=""></p><br>

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

<br><p align="center"><img src="https://github.com/user-attachments/assets/13c39a56-b2fb-4ac4-85a8-4d2f3a11d07a" alt=""></p><br>

Redémarrez le poste client et tentez d’installer un fichier .msi avec un compte utilisateur standard.



# 4- LA GPO des périphériques amovibles :
<span id="peripherique"></span> 

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

1. **Refuser l'accès à toutes les classes de stockage amovible**
   - Double-cliquer sur **"Toutes les classes de stockage amovible: refuser tous les accès"**
   - Sélectionner **Activé**
<br><p align="center"><img src="https://github.com/user-attachments/assets/68f8e11c-5953-4ed9-9bbd-62d6dfb742e7" alt=""></p><br>

2. **Refuser l'accès en lecture aux disques amovibles**
   - Double-cliquer sur **"Refuser l'accès en lecture aux disques amovibles"**
   - Sélectionner **Activé**
<br><p align="center"><img src="https://github.com/user-attachments/assets/77958c51-18af-44d2-ab20-7196241c1471" alt=""></p><br>

<br><p align="center"><img src="https://github.com/user-attachments/assets/e436c18d-8c67-468b-ba57-92b23f26bb65" alt=""></p><br>

3. **Refuser l'accès à l'exécution aux périphériques de stockage amovibles**
   - Double-cliquer sur **"Refuser l'accès à l'exécution aux périphériques de stockage amovibles"**
   - Sélectionner **Activé**
<br><p align="center"><img src="https://github.com/user-attachments/assets/2b015581-8def-4cd4-8973-81b9fca86705" alt=""></p><br>

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


# 5- LA GPO pour configurer un mot de passe en sortie d'ecran de veille :
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

