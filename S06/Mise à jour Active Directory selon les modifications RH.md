
# Tutoriel : Mise à jour Active Directory selon les modifications RH

## Prérequis
- Accès à un compte administrateur Active Directory
- Outils nécessaires :
  - PowerShell avec module Active Directory
  - Console de gestion Active Directory
  - Fichier `S06_EcoTechSolutions.xlsx`

---

## 1. Intégration des nouveaux utilisateurs
### Étapes :
Nous avons comparé les deux fichiers CSV afin d'identifier les nouveaux arrivants et de créer un nouveau fichier CSV qui servira à l'ajout grâce à ce script PowerShell.

<pre> 
$csvPath = "C:\Users\Administrator.WINSERV1\Desktop\CSV\nouveaux_utilisateurs_s06.csv"

# Importation des données
$users = Import-Csv -Path $csvPath -Delimiter ","

foreach ($user in $users) {
    $firstName = $user.Prenom
    $lastName = $user.Nom
    $fullName = "$firstName $lastName"
    $samAccountName = "$($firstName[0])$lastName" -replace "[^a-zA-Z0-9]", ""
    $department = $user.Departement
    $service = $user.Service
    $site = $user.Site

    # Chemin des OUs
    $ouSite = "OU=$site,DC=ecotechsolutions,DC=com"
    $ouUsers = "OU=Utilisateurs,$ouSite"
    $ouDepartment = "OU=$department,$ouUsers"
    $ouService = "OU=$service,$ouDepartment"
    $ouGroupes = "OU=Groupes,$ouSite"

    # Vérifier et créer les OUs si elles n'existent pas
    try {
        if (-not (Get-ADOrganizationalUnit -Filter {Name -eq $site} -ErrorAction Stop)) {
            New-ADOrganizationalUnit -Name $site -Path "DC=ecotechsolutions,DC=com" -ErrorAction Stop
            Write-Host "OU $site créée."
        } else {
            Write-Host "OU $site existe déjà."
        }
    } catch {
        Write-Host "Erreur lors de la création de l'OU $site : $_"
    }
   
    try {
        if (-not (Get-ADOrganizationalUnit -Filter {Name -eq "Utilisateurs" -and DistinguishedName -like "*$ouSite"} -ErrorAction Stop)) {
            New-ADOrganizationalUnit -Name "Utilisateurs" -Path $ouSite -ErrorAction Stop
            Write-Host "OU Utilisateurs créée."
        }
    } catch {
        Write-Host "Erreur lors de la création de l'OU Utilisateurs : $_"
    }
   
    try {
        if (-not (Get-ADOrganizationalUnit -Filter {Name -eq $department -and DistinguishedName -like "*$ouUsers"} -ErrorAction Stop)) {
            New-ADOrganizationalUnit -Name $department -Path $ouUsers -ErrorAction Stop
            Write-Host "OU $department créée."
        }
    } catch {
        Write-Host "Erreur lors de la création de l'OU $department : $_"
    }
   
    try {
        if (-not (Get-ADOrganizationalUnit -Filter {Name -eq $service -and DistinguishedName -like "*$ouDepartment"} -ErrorAction Stop)) {
            New-ADOrganizationalUnit -Name $service -Path $ouDepartment -ErrorAction Stop
            Write-Host "OU $service créée."
        }
    } catch {
        Write-Host "Erreur lors de la création de l'OU $service : $_"
    }

    # Vérifier si l'utilisateur existe déjà
    if (Get-ADUser -Filter {SamAccountName -eq $samAccountName} -SearchBase $ouService -ErrorAction SilentlyContinue) {
        Write-Host "Utilisateur $samAccountName existe déjà, passage..."
    } else {
        Write-Host "Création de l'utilisateur : $fullName dans l'OU $ouService"
        New-ADUser -SamAccountName $samAccountName -UserPrincipalName "$samAccountName@ecotechsolutions.com" `
            -Name $fullName -GivenName $firstName -Surname $lastName -Department $department `
            -Path $ouService -Enabled $true -AccountPassword (ConvertTo-SecureString "Azerty1*" -AsPlainText -Force) -ErrorAction SilentlyContinue
        if ($?) {
            Set-ADUser -Identity $samAccountName -Description $user.fonction -ErrorAction SilentlyContinue
            if ($?) { Write-Host "Utilisateur $fullName créé avec succès." } else { Write-Host "Erreur lors de la mise à jour de l'utilisateur $fullName." }
        } else {
            Write-Host "Erreur lors de la création de l'utilisateur $fullName."
        }
    }

    # Gestion des groupes
    $groupName = "grp-$department"
    $securityGroup = "grp-$service"

    if (-not (Get-ADGroup -Filter {Name -eq $groupName} -ErrorAction SilentlyContinue)) {
        New-ADGroup -Name $groupName -GroupScope Global -Path $ouGroupes -ErrorAction SilentlyContinue
        if ($?) { Write-Host "Groupe $groupName créé." } else { Write-Host "Erreur lors de la création du groupe $groupName." }
    }
    if (-not (Get-ADGroupMember -Identity $groupName -ErrorAction SilentlyContinue | Where-Object { $_.SamAccountName -eq $samAccountName })) {
        Add-ADGroupMember -Identity $groupName -Members $samAccountName -ErrorAction SilentlyContinue
        if ($?) { Write-Host "Utilisateur $samAccountName ajouté au groupe $groupName." } else { Write-Host "Erreur lors de l'ajout de $samAccountName au groupe $groupName." }
    }

    if (-not (Get-ADGroup -Filter {Name -eq $securityGroup} -ErrorAction SilentlyContinue)) {
        New-ADGroup -Name $securityGroup -GroupScope Global -Path $ouGroupes -ErrorAction SilentlyContinue
        if ($?) { Write-Host "Groupe $securityGroup créé." } else { Write-Host "Erreur lors de la création du groupe $securityGroup." }
    }
    if (-not (Get-ADGroupMember -Identity $securityGroup -ErrorAction SilentlyContinue | Where-Object { $_.SamAccountName -eq $samAccountName })) {
        Add-ADGroupMember -Identity $securityGroup -Members $samAccountName -ErrorAction SilentlyContinue
        if ($?) { Write-Host "Utilisateur $samAccountName ajouté au groupe $securityGroup." } else { Write-Host "Erreur lors de l'ajout de $samAccountName au groupe $securityGroup." }
    }
}

</pre> 

## 📌 2. Modification des informations des utilisateurs
1. **Ouvrir la fiche d'un utilisateur** :
   - Aller dans la **OU concernée**.
   - Double-cliquer sur l’utilisateur.
2. **Modifier les informations** :
   - **Onglet Général** : Modifier le **nom** (mariage, changement de responsable, etc.).
     - Modifier le **nom** (ex. Alexis Fisher → Alexis Fisher-Drumond).
   - Cliquer sur **Appliquer**, puis **OK**.
![Mise à jour AD](https://raw.githubusercontent.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/main/Ressources/Images/S06/Mise%20%C3%A0%20jour%20Active%20Directory%20selon%20les%20modifications%20RH/AD_ModifNom_suiteMariage.png)

---

## 📌 3. Gestion des départs et désactivation des comptes
1. **Trouver les utilisateurs quittant l’entreprise** :
   - Aller dans l’OU des employés.
   - Double-cliquer sur l’utilisateur concerné.
2. **Désactiver le compte** :
   - **Onglet Compte** → Cocher **"Désactiver le compte"**.
   - Valider avec **OK**.
3. **Déplacer les comptes désactivés** (optionnel) :
   - Créer une **OU "Anciens employés"** (si non existante).
   - Déplacer les comptes désactivés par **glisser-déposer** ou :
     - Clic droit → **Déplacer** → Sélectionner la OU cible → **OK**.
![Désactivation Compte AD](https://raw.githubusercontent.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/main/Ressources/Images/S06/Mise%20%C3%A0%20jour%20Active%20Directory%20selon%20les%20modifications%20RH/AD_D%C3%A9sactivation_Compte.png)

---

## 📌 4. Modification des noms et services

### 🏢 Changement du nom d’un département  
**(Finance et Comptabilité → Direction Financière)**
1. Aller dans la **OU contenant les départements**.
2. **Renommer la OU** :
   - Clic droit sur `Finance et Comptabilité` → **Renommer** → `Direction Financière`.
![Changement de Nom Service AD](https://raw.githubusercontent.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/main/Ressources/Images/S06/Mise%20%C3%A0%20jour%20Active%20Directory%20selon%20les%20modifications%20RH/AD_Chgt_NomService.png)


### ❌ Suppression d’un service  
**(Fiscalité fusionné avec Finance)**
1. **Supprimer la OU "Fiscalité"** :
   - Clic droit sur la OU `Fiscalité` → **Supprimer**.
2. **Déplacer les utilisateurs vers "Finance"** :
   - Aller dans `Fiscalité`, sélectionner tous les comptes utilisateurs (`Ctrl + clic` pour multi-sélection).
   - Clic droit → **Déplacer** → Choisir la OU `Finance` → **OK**.
![Suppression Fiscalité AD](https://raw.githubusercontent.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/main/Ressources/Images/S06/Mise%20%C3%A0%20jour%20Active%20Directory%20selon%20les%20modifications%20RH/AD_Suppression_Fiscalit%C3%A9.png)

---

## 📌 5. Gestion de la nouvelle hiérarchie

### ✅ Ajout d’Iko Loubert comme responsable B2B
1. Aller dans la **OU "Service Commercial"**.
2. **Créer le compte utilisateur d’Iko Loubert** (cf. Étape 2).
3. **Définir son poste et responsable hiérarchique** :
   - **Onglet Organisation** :
     - **Poste** : Responsable B2B.
     - **Manager** : Sélectionner son supérieur dans AD.
![AD Iko Loubert](https://raw.githubusercontent.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/main/Ressources/Images/S06/Mise%20%C3%A0%20jour%20Active%20Directory%20selon%20les%20modifications%20RH/AD_IkoLoubert.png)


### 🔄 Gestion des subalternes suite au départ de Lana Wong
1. Ouvrir **Marina Brun** (nouvelle Directrice Commerciale).
2. Aller dans **Organisation** → **Définir comme responsable** des employés de Lana Wong.
![AD Marina Brun](https://raw.githubusercontent.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/main/Ressources/Images/S06/Mise%20%C3%A0%20jour%20Active%20Directory%20selon%20les%20modifications%20RH/AD_MarinaBrun.png)

---

