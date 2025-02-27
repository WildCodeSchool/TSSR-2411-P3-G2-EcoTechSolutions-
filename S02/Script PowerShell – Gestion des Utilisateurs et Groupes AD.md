
# 📌 Script PowerShell – Gestion des Utilisateurs et Groupes AD  

## 📝 Description  
Ce script PowerShell permet d'automatiser la gestion des utilisateurs et des groupes Active Directory (AD) en fonction des informations contenues dans un fichier CSV.  

## 🔹 Fonctionnalités  

### 1️⃣ Importation des utilisateurs depuis un fichier CSV  
- Charge les données depuis un fichier CSV (`s01_EcoTechSolutions.csv`), qui contient des informations telles que :  
  - **Prénom**  
  - **Nom**  
  - **Département**  
  - **Service**  
  - **Site**  

### 2️⃣ Création ou vérification des utilisateurs  
- Génère un `samAccountName` basé sur l'initiale du prénom et le nom de famille.  
- Vérifie si l'utilisateur existe déjà dans AD :  
  - ✅ **S'il existe** → Passage à l'utilisateur suivant.  
  - ❌ **S'il n'existe pas** → Création de l'utilisateur avec :  
    - Un mot de passe par défaut (`Azerty1*`).  
    - Ajout dans l'OU appropriée en fonction du site.  

### 3️⃣ Gestion des groupes de département et de service  
- Vérifie si un **groupe de département** (`grp-Département`) existe :  
  - ✅ **S'il existe** → Passage à l'étape suivante.  
  - ❌ **S'il n'existe pas** → Création du groupe dans l'OU correspondante.  
- Vérifie si un **groupe de service** (`grp-Service`) existe et si l'utilisateur en est déjà membre.  

### 4️⃣ Ajout des utilisateurs aux groupes  
- Ajoute chaque utilisateur au **groupe correspondant à son département**.  
- Ajoute chaque utilisateur au **groupe de sécurité correspondant à son service**, **si le groupe existe déjà**.  

## 🎯 Objectif du script  
Ce script facilite la gestion des utilisateurs Active Directory en garantissant que :  
✅ Tous les utilisateurs du fichier CSV sont présents dans l'AD.  
✅ Les utilisateurs sont correctement assignés à leurs groupes respectifs.  
✅ Aucun utilisateur ou groupe existant n'est dupliqué.  

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
    $groupName = "grp-$department"
    $securityGroup = "grp-$service"
   
    # Vérifier si l'utilisateur existe déjà
    if (Get-ADUser -Filter {SamAccountName -eq $samAccountName}) {
        Write-Host "Utilisateur $samAccountName existe déjà, passage..."
    } else {
        # Création de l'utilisateur
        New-ADUser -SamAccountName $samAccountName -UserPrincipalName "$samAccountName@ecotechsolutions.com" \
            -Name $fullName -GivenName $firstName -Surname $lastName -Department $department \
            -Path "OU=Utilisateurs,OU=$($user.Site),DC=ecotechsolutions,DC=com" -Enabled $true -AccountPassword (ConvertTo-SecureString "Azerty1*" -AsPlainText -Force) \
            -PassThru | Set-ADUser -Description $user.fonction
    }
   
    # Vérifier si le groupe de département existe déjà
    if (Get-ADGroup -Filter {Name -eq $groupName}) {
        Write-Host "Groupe $groupName existe déjà, passage..."
    } else {
        New-ADGroup -Name $groupName -GroupScope Global -Path "OU=Groupes,OU=$($user.Site),DC=ecotechsolutions,DC=com"
    }
   
    # Vérifier si l'utilisateur est déjà membre du groupe avant de l'ajouter
    if (-not (Get-ADGroupMember -Identity $groupName | Where-Object { $_.SamAccountName -eq $samAccountName })) {
        Add-ADGroupMember -Identity $groupName -Members $samAccountName
    }
   
    # Vérifier si le groupe de sécurité existe déjà
    if (Get-ADGroup -Filter {Name -eq $securityGroup}) {
        # Vérifier si l'utilisateur est déjà membre du groupe de sécurité
        if (-not (Get-ADGroupMember -Identity $securityGroup | Where-Object { $_.SamAccountName -eq $samAccountName })) {
            Add-ADGroupMember -Identity $securityGroup -Members $samAccountName
        }
    } else {
        Write-Host "Groupe $securityGroup non trouvé, passage..."
    }
}
</pre> 
