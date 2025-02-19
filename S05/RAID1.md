# 📌 Configuration d’un RAID 1 (Mirroring) sur Windows Server 2022 (GUI) sous Proxmox

## 🛠️ Prérequis
- Un **serveur Proxmox** avec une **VM Windows Server 2022** installée.
- **Deux disques virtuels supplémentaires** attachés à la VM.
- Un accès **administrateur** sur Windows Server 2022.


---

## 📖 Étape 1 : Ajouter des disques à la VM sous Proxmox
1. **Accéder à l’interface Proxmox** via un navigateur :  
   `https://[2a01:4f8:222:2d0e::2]:8006/`
2. Sélectionner la **VM Windows Server 2022**.
3. Aller dans **Matériel** > **Ajouter** > **Disque**.
4. Configurer les paramètres :
   - **Stockage** : Choisir l’emplacement du disque.
   - **Taille** : Spécifier une taille identique pour les 2 disques (ex. 32 Go).
   - **Bus/Contrôleur** : `VirtIO` (recommandé) ou `SATA` si VirtIO n’est pas installé.
   - **Cocher "Ignorer l’échec du disque"** si disponible.
5. **Répéter l’opération** pour ajouter un **deuxième disque**.
   
| Ajout du premier disque | Ajout du deuxième disque |
|-------------------------|-------------------------|
| ![Ajout disque 1](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S05/RAID/1_Ajoutdisque1.PNG?raw=true) | ![Ajout disque 2](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S05/RAID/2_Ajoutdisque2.PNG?raw=true) |


---

## 📖 Étape 2 : Vérifier la présence des disques dans Windows
1. **Démarrer la VM** et ouvrir le **Gestionnaire de disques** :
   - `Démarrer` > Rechercher `diskmgmt.msc` > `Entrée`
2. Deux nouveaux disques **Non alloués** doivent apparaître.
3. Si les disques sont **Non initialisés** :
   - **Clic droit** > **Initialiser le disque**.
   - Choisir **GPT** (recommandé) ou **MBR**.

![Liste des disques durs](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S05/RAID/3_ListeDesDD.PNG?raw=true)


---

## 📖 Étape 3 : Créer un RAID 1 (Mirroring) sous Windows Server 2022
1. **Clic droit** sur un des deux disques > **Nouveau volume en miroir**.
2. Suivre l’assistant :
   - **Suivant**.
   - **Ajouter le deuxième disque** pour le RAID 1.
   - **Attribuer une lettre de lecteur** (ex. `E:`).
   - **Choisir NTFS** et **activer le formatage rapide**.
   - **Donner un nom au volume** (ex. `RAID1_DISK`).
3. **Finaliser** et attendre la synchronisation des disques.

| Création du volume miroir | Attribution de la lettre du lecteur |
|--------------------------|------------------------------------|
| ![Nouveau volume miroir](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S05/RAID/4_NouveauVolumeMiroir.PNG?raw=true) | ![Attribution lettre lecteur](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S05/RAID/5_AttributionLettreLecteur.PNG?raw=true) |

| Formatage NTFS rapide | Finalisation et synchronisation |
|----------------------|--------------------------------|
| ![Formatage NTFS rapide](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S05/RAID/6_NTFSFormatageRapide.PNG?raw=true) | ![Finalisation et synchronisation](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S05/RAID/7_FinalisationSynchronisation.PNG?raw=true) |


---

## 📖 Étape 4 : Vérifier le RAID 1
1. Retourner dans **Gestionnaire de disques** (`diskmgmt.msc`).
2. Vérifier que les deux disques sont en **"Miroir"** et synchronisés.
3. Aller dans `Ce PC` (`Win + E`) pour voir le volume RAID.

| Liste des disques après configuration RAID | Vérification du RAID 1 |
|------------------------------------------|------------------------|
| ![Liste des disques RAID](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S05/RAID/8_ListeDesDDRAID.PNG?raw=true) | ![Vérification RAID 1](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S05/RAID/9_VerificationRAID1.PNG?raw=true) |


---

## 🎯 Tests et validation
- **Tester un disque** :  
  Éteindre la VM, supprimer un disque sous **Proxmox**, redémarrer la VM et vérifier l'accès aux données.
- **Restaurer** le disque manquant :
  1. Ajouter un nouveau disque sous **Proxmox**.
  2. Dans **Gestionnaire de disques**, resynchroniser le miroir.

---

