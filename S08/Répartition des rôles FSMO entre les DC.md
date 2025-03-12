# Répartir les rôles FSMO entre les DC

### a) Les 5 rôles FSMO

1. **Schema Master**  
2. **Domain Naming Master**  
3. **PDC Emulator**  
4. **RID Master**  
5. **Infrastructure Master**

Initialement, tous les rôles sont généralement sur le **premier DC** créé. Il est conseillé de les **répartir** pour éviter qu’un DC unique ne cumule tous les rôles (et représenterait un point de panne majeur).

### b) Exemple de répartition

- **DC1 : SRV-AD-01 :**  
  - Schema Master  
  - Domain Naming Master  

- **DC2 : SRV-AD-02-TEST **  
  - PDC Emulator  
  - RID Master  

- **DC3 : SRV-AD-03-TEST **  
  - Infrastructure Master  

Cette configuration est courante, mais vous pouvez bien sûr adopter une autre répartition.

### c) Transférer ou “basculer” les rôles FSMO

> **Note :** Pour transférer un rôle, assurez-vous d’être **connecté sur le DC** où vous voulez **détenir** le rôle en question.

#### Méthode 1 : via les consoles MMC

1. **Ouvrir la console appropriée** :  
   - *Utilisateurs et ordinateurs Active Directory* (pour RID, PDC, Infrastructure)  
   - *Domaines et approbations Active Directory* (pour Domain Naming Master)  
   - *Schéma Active Directory* (pour le Schema Master)  

   *(Pour le Schéma, il faut parfois l'enregistrer via `regsvr32 schmmgmt.dll`)*

2. **Menu “Opérations maîtres…”** (ou “Operations Master…” en anglais).  
3. Dans la fenêtre qui s’ouvre, cliquez sur **“Modifier”** ou **“Changer…”** pour **basculer** le rôle vers le DC de votre choix.

#### Méthode 2 : via PowerShell

1. **Ouvrez PowerShell** (en Administrateur) sur le DC qui va recevoir les rôles.  
2. Utilisez la cmdlet :
   ```powershell
   Move-ADDirectoryServerOperationMasterRole -Identity "NouveauDC" -OperationMasterRole PDCEmulator,RIDMaster,InfrastructureMaster

