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

---

## 📖 Étape 2 : Vérifier la présence des disques dans Windows
1. **Démarrer la VM** et ouvrir le **Gestionnaire de disques** :
   - `Démarrer` > Rechercher `diskmgmt.msc` > `Entrée`
2. Deux nouveaux disques **Non alloués** doivent apparaître.
3. Si les disques sont **Non initialisés** :
   - **Clic droit** > **Initialiser le disque**.
   - Choisir **GPT** (recommandé) ou **MBR**.

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

---

## 📖 Étape 4 : Vérifier le RAID 1
1. Retourner dans **Gestionnaire de disques** (`diskmgmt.msc`).
2. Vérifier que les deux disques sont en **"Miroir"** et synchronisés.
3. Aller dans `Ce PC` (`Win + E`) pour voir le volume RAID.

---

## 🎯 Tests et validation
- **Tester un disque** :  
  Éteindre la VM, supprimer un disque sous **Proxmox**, redémarrer la VM et vérifier l'accès aux données.
- **Restaurer** le disque manquant :
  1. Ajouter un nouveau disque sous **Proxmox**.
  2. Dans **Gestionnaire de disques**, resynchroniser le miroir.

---

