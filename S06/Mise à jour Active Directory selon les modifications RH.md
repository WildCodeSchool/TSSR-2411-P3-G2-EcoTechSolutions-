
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

## 2. Modifications des informations des utilisateurs
### Changement de nom des collaborateurs mariés
1. Modifier le nom d’un utilisateur :
   ```powershell
   Set-ADUser -Identity "jdoe" -GivenName "Jane" -Surname "Dupont" -DisplayName "Jane Dupont" -UserPrincipalName "jdupont@domaine.local"
   ```
2. Vérifier la modification :
   ```powershell
   Get-ADUser -Identity "jdupont"
   ```

---

## 3. Changement des départements et services
### Modification du nom du département "Finance et Comptabilité"
1. Modifier le département dans AD pour tous les employés concernés :
   ```powershell
   Get-ADUser -Filter "Department -eq 'Finance et Comptabilité'" | ForEach-Object {
       Set-ADUser -Identity $_.SamAccountName -Department "Direction financière"
   }
   ```

### Suppression du service "Fiscalité" et transfert des employés
1. Modifier les utilisateurs pour qu'ils appartiennent désormais à "Finance" :
   ```powershell
   Get-ADUser -Filter "Department -eq 'Direction financière' -and Title -eq 'Fiscalité'" | ForEach-Object {
       Set-ADUser -Identity $_.SamAccountName -Title "Finance"
   }
   ```

---

## 4. Gestion des départs
### Désactivation des comptes AD
1. Désactiver un compte utilisateur :
   ```powershell
   Disable-ADAccount -Identity "lancien"
   ```
2. Désactiver plusieurs comptes à partir d’une liste :
   ```powershell
   Import-Csv "depart_utilisateurs.csv" | ForEach-Object {
       Disable-ADAccount -Identity $_.SamAccountName
   }
   ```
3. Vérifier les comptes désactivés :
   ```powershell
   Get-ADUser -Filter "Enabled -eq 'False'"
   ```

### Suppression des données associées
- Déplacer les fichiers vers un stockage d’archive si nécessaire.
- Supprimer les boîtes mails et accès aux ressources partagées.

---

## 5. Mise à jour de l'organigramme
### Changement de directrice commerciale
1. Modifier le manager de tous les subalternes de Lana Wong vers Marina Brun :
   ```powershell
   Get-ADUser -Filter "Manager -eq 'Lana Wong'" | ForEach-Object {
       Set-ADUser -Identity $_.SamAccountName -Manager "Marina Brun"
   }
   ```

### Ajout d'Iko Loubert comme responsable B2B
1. Définir son rôle dans l'organigramme :
   ```powershell
   Set-ADUser -Identity "iloubert" -Manager "Marina Brun"
   ```
2. Vérifier la mise à jour :
   ```powershell
   Get-ADUser -Identity "iloubert" -Properties Manager
   ```

---

## Conclusion
En suivant ces étapes, l’Active Directory sera mis à jour conformément aux nouvelles directives RH. Pensez à toujours vérifier les modifications avant validation définitive.
```

