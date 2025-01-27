# S01 USER GUIDE
## 1) Schéma réseau
| ![Infra actuelle](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/R%C3%A9seau/InfraActuelle_EcoTechSolutions.png) | ![Image 2](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/R%C3%A9seau/Infra_%C3%A0_venir.png) |
|-------------------------------|-------------------------------|

## 2) Convention de nommage

### 1. Les Compte utilisateurs
- **Le Format des noms d’utilisateur** : La structure sera sous la forme "Prénom.Nom" (Exemple : Camille.bataille)
- **Les Comptes administrateurs** : La structure sera sous la forme "admin-[NomDépartement]" (Exemple : admin-rh)
- **Les Comptes génériques ou de testStructure** : La structure sera sous la forme "test-[Numéro]" (Exemple : test-01)

**Les Restrictions :**
- Tous les noms sont en minuscules.
- Pas de caractères spéciaux ni d’accents.
- Longueur maximale : 20-25 caractères.

---

### 2. Les Groupes d’utilisateurs
- **Le Format des noms de groupes** : La structure sera sous la forme "grp-NomDépartement-Rôle" (Exemple : grp-dev-utilisateurs)

**Règles générales**
- Utiliser des noms explicites pour décrire le rôle ou l’accès.
- Grouper les utilisateurs par fonction pour une gestion des droits simplifiée.


---
### 3. Les Équipements Informatiques
- **Les Équipements Informatiques** : La structure sera sous la forme  "PC-NomDépartement-InitialesUtilisateur" (Exemple : PC-Finance-JD (PC de Jean Dupont du département Finance)
- **Les Serveurs** : La structure sera sous la forme "SRV-Fonction-Numéro" (Exemple : SRV-AD-01 = Premier serveur Active Directory)
- **Le Matériel Réseau** : La structure sera sous la forme "Type-Localisation-Numéro"(SW-Bordeaux-01 =Premier switch du site de Bordeaux)
*Types courants* :
SW : Switch
RT : Routeur
FW : Pare-feu
AP : Point d’accès Wi-Fi

---
### 4. Les Ressources Partagées
- **Les Dossiers Partagés** : La structure sera sous la forme "NomDépartement-Fonction/Rôle"" (Exemple : DSI-Projets)

---
# 5. Les Réseaux
- **Nom des SSID Wi-Fi** : La structure sera sous la forme `EcoTech-Usage` (Exemple : `EcoTech-Interne` pour le Wi-Fi interne).  
- **Plan d’adressage IP** :  
  - **Plage IP** : `10.10.0.0/16`  
  - **Masque** : `255.255.0.0`  
  - **Passerelle par défaut** : `10.10.255.254`  
  - **Adresse IP DNS** : `10.10.255.254`  

## Attribution par département

| Département/Service          | Sous-réseau       | Plage d'adresses utilisables | Masque              | Exemple d'adresses attribuées    |
|------------------------------|-------------------|------------------------------|---------------------|-----------------------------------|
| **Communication**            | `10.10.1.0/24`   | `10.10.1.1 - 10.10.1.254`    | `255.255.255.0`     | Postes : `10.10.1.2-10.10.1.50`  |
| **Développement**            | `10.10.2.0/24`   | `10.10.2.1 - 10.10.2.254`    | `255.255.255.0`     | Postes : `10.10.2.2-10.10.2.50`  |
| **Direction**                | `10.10.3.0/24`   | `10.10.3.1 - 10.10.3.254`    | `255.255.255.0`     | Postes : `10.10.3.2-10.10.3.10`  |
| **Ressources Humaines (RH)** | `10.10.4.0/24`   | `10.10.4.1 - 10.10.4.254`    | `255.255.255.0`     | Postes : `10.10.4.2-10.10.4.20`  |
| **DSI**                      | `10.10.5.0/24`   | `10.10.5.1 - 10.10.5.254`    | `255.255.255.0`     | Postes : `10.10.5.2-10.10.5.20`  |
| **Finance et Comptabilité**  | `10.10.6.0/24`   | `10.10.6.1 - 10.10.6.254`    | `255.255.255.0`     | Postes : `10.10.6.2-10.10.6.50`  |
| **Service Commercial**       | `10.10.7.0/24`   | `10.10.7.1 - 10.10.7.254`    | `255.255.255.0`     | Postes : `10.10.7.2-10.10.7.50`  |


---
### 6. Les Sauvegardes et les Fichiers
 - **Format des fichiers** : La structure sera sous la forme "NomDépartement_TypeDeDocument_Date" (Exemple : RH_SuiviEmbauches_20250101)
 - **Dossiers de Sauvegarde** : La structure sera sous la forme "Backup-NomDépartement-Fréquence" (Exemple : Backup-DSI-Daily)

---
### 7. Exceptions et Validation
- Toute exception à cette convention doit être validée par le DSI d’EcoTech Solutions.
- Les exceptions doivent être documentées et justifiées pour garantir la traçabilité.

---
  
## 3) Matériel et dispositifs réseau**


## 4) Table de Routage
