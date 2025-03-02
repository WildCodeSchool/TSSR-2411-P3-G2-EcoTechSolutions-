
# 📌 Script PowerShell – Gestion Automatisée des Utilisateurs et Groupes AD  

## 📝 Description  
Ce script PowerShell permet d'automatiser l'importation et la gestion des utilisateurs dans Active Directory (AD) à partir d'un fichier CSV. Il assure la création des **unités organisationnelles (OUs)**, des **comptes utilisateurs**, ainsi que des **groupes** en fonction des informations fournies.  

## 🔹 Fonctionnalités  

### 1️⃣ Importation des utilisateurs depuis un fichier CSV  
- Charge les données depuis un fichier CSV (`s01_EcoTechSolutions.csv`), contenant les champs suivants :  
  - **Prénom** (`Prenom`)  
  - **Nom** (`Nom`)  
  - **Département** (`Departement`)  
  - **Service** (`Service`)  
  - **Site** (`Site`)  

### 2️⃣ Vérification et création des OUs  
- Construit une hiérarchie d'**unités organisationnelles (OUs)** basée sur les informations du fichier :  
  - `OU=Site`  
  - `OU=Utilisateurs,OU=Site`  
  - `OU=Département,OU=Utilisateurs,OU=Site`  
  - `OU=Service,OU=Département,OU=Utilisateurs,OU=Site`  
  - `OU=Groupes,OU=Site`  
- Vérifie si chaque OU existe déjà :  
  - ✅ **Si elle existe** → Passage à l'étape suivante.  
  - ❌ **Si elle n'existe pas** → Création de l'OU.  

### 3️⃣ Création ou vérification des utilisateurs  
- Génère un `samAccountName` basé sur **l'initiale du prénom et le nom de famille**.  
- Vérifie si l'utilisateur existe déjà dans Active Directory :  
  - ✅ **S'il existe** → L'utilisateur est ignoré.  
  - ❌ **S'il n'existe pas** → Création du compte utilisateur avec :  
    - Un **nom d'utilisateur unique**.  
    - Un **mot de passe par défaut** (`Azerty1*`).  
    - L'activation immédiate du compte.  
    - L'ajout à l'**OU correspondant à son service**.  
    - Mise à jour de la **description avec la fonction de l'utilisateur**.  

### 4️⃣ Gestion des groupes de département et de service  
- Vérifie si un **groupe de département** (`grp-Département`) existe :  
  - ✅ **S'il existe** → Passage à l'étape suivante.  
  - ❌ **S'il n'existe pas** → Création du groupe dans l'OU des groupes.  
- Vérifie si un **groupe de service** (`grp-Service`) existe :  
  - ✅ **S'il existe** → Passage à l'étape suivante.  
  - ❌ **S'il n'existe pas** → Création du groupe.  

### 5️⃣ Ajout des utilisateurs aux groupes  
- Ajoute chaque utilisateur au **groupe correspondant à son département** (`grp-Département`).  
- Ajoute chaque utilisateur au **groupe de sécurité correspondant à son service** (`grp-Service`).  

## 🎯 Objectif du script  
Ce script facilite la gestion des utilisateurs Active Directory en garantissant que :  
✅ Tous les utilisateurs du fichier CSV sont présents dans l'AD.  
✅ Les unités organisationnelles sont créées dynamiquement en fonction des besoins.  
✅ Les utilisateurs sont correctement assignés à leurs **groupes respectifs**.  
✅ Aucun **compte utilisateur ou groupe existant** n'est dupliqué.  


---  
<pre> 
$csvPath = "C:\Users\Administrator.WINSERV1\Desktop\CSV\s01_EcoTechSolutions.csv"

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

Voici une autre version du script mis à jour avec la gestion des doublons du SAMAccountName. Si un SAMAccountName est déjà pris, il sera incrémenté avec un chiffre à la fin (ex. jdoe, jdoe1, jdoe2, etc.).

<pre> 
$csvPath = "C:\Users\Administrator.WINSERV1\Desktop\CSV\s01_EcoTechSolutions.csv"

# Importation des données
$users = Import-Csv -Path $csvPath -Delimiter ","

foreach ($user in $users) {
    $firstName = $user.Prenom
    $lastName = $user.Nom
    $fullName = "$firstName $lastName"
    $baseSamAccountName = "$($firstName[0])$lastName" -replace "[^a-zA-Z0-9]", ""
    $samAccountName = $baseSamAccountName
    $counter = 1

    # Vérifier si le samAccountName existe déjà et le modifier si nécessaire
    while (Get-ADUser -Filter {SamAccountName -eq $samAccountName} -ErrorAction SilentlyContinue) {
        $samAccountName = "$baseSamAccountName$counter"
        $counter++
    }

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
    function Ensure-OUExists {
        param ($ouPath, $ouName, $parentPath)
        if (-not (Get-ADOrganizationalUnit -Filter {Name -eq $ouName -and DistinguishedName -like "*$parentPath"} -ErrorAction SilentlyContinue)) {
            New-ADOrganizationalUnit -Name $ouName -Path $parentPath -ErrorAction Stop
            Write-Host "OU $ouName créée."
        }
    }

    Ensure-OUExists -ouPath $ouSite -ouName $site -parentPath "DC=ecotechsolutions,DC=com"
    Ensure-OUExists -ouPath $ouUsers -ouName "Utilisateurs" -parentPath $ouSite
    Ensure-OUExists -ouPath $ouDepartment -ouName $department -parentPath $ouUsers
    Ensure-OUExists -ouPath $ouService -ouName $service -parentPath $ouDepartment

    # Création de l'utilisateur
    Write-Host "Création de l'utilisateur : $fullName avec SAMAccountName: $samAccountName dans l'OU $ouService"
    New-ADUser -SamAccountName $samAccountName -UserPrincipalName "$samAccountName@ecotechsolutions.com" `
        -Name $fullName -GivenName $firstName -Surname $lastName -Department $department `
        -Path $ouService -Enabled $true -AccountPassword (ConvertTo-SecureString "Azerty1*" -AsPlainText -Force) -ErrorAction SilentlyContinue
    if ($?) {
        Set-ADUser -Identity $samAccountName -Description $user.fonction -ErrorAction SilentlyContinue
        Write-Host "Utilisateur $fullName créé avec succès avec le SAMAccountName : $samAccountName."
    } else {
        Write-Host "Erreur lors de la création de l'utilisateur $fullName."
    }

    # Gestion des groupes
    function Ensure-GroupExists {
        param ($groupName, $ouPath)
        if (-not (Get-ADGroup -Filter {Name -eq $groupName} -ErrorAction SilentlyContinue)) {
            New-ADGroup -Name $groupName -GroupScope Global -Path $ouPath -ErrorAction SilentlyContinue
            Write-Host "Groupe $groupName créé."
        }
    }

    function Add-UserToGroup {
        param ($groupName, $samAccountName)
        if (-not (Get-ADGroupMember -Identity $groupName -ErrorAction SilentlyContinue | Where-Object { $_.SamAccountName -eq $samAccountName })) {
            Add-ADGroupMember -Identity $groupName -Members $samAccountName -ErrorAction SilentlyContinue
            Write-Host "Utilisateur $samAccountName ajouté au groupe $groupName."
        }
    }

    $groupName = "grp-$department"
    $securityGroup = "grp-$service"

    Ensure-GroupExists -groupName $groupName -ouPath $ouGroupes
    Add-UserToGroup -groupName $groupName -samAccountName $samAccountName

    Ensure-GroupExists -groupName $securityGroup -ouPath $ouGroupes
    Add-UserToGroup -groupName $securityGroup -samAccountName $samAccountName
}

</pre> 

