
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
1. Ouvrir PowerShell en mode administrateur.
2. Utiliser la commande suivante pour ajouter un utilisateur :
   ```powershell
   New-ADUser -Name "Iko Loubert" -GivenName "Iko" -Surname "Loubert" -SamAccountName "iloubert" -UserPrincipalName "iloubert@domaine.local" -Department "Service Commercial" -Title "Responsable B2B" -Path "OU=Service Commercial,OU=Utilisateurs,DC=domaine,DC=local" -Enabled $true -PasswordNeverExpires $true
   ```
3. Vérifier que l'utilisateur a bien été ajouté avec :
   ```powershell
   Get-ADUser -Identity "iloubert"
   ```
4. Ajouter les permissions et les groupes nécessaires avec :
   ```powershell
   Add-ADGroupMember -Identity "Groupe_B2B" -Members "iloubert"
   ```

---

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

