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
### 5. Les Réseaux
- **Nom des SSID Wi-Fi** : La structure sera sous la forme "EcoTech-Usage" (Exemple EcoTech-Interne = Wi-Fi interne)
- **Plan d’adressage IP** :
      - **Plage IP** : 10.10.8.0/24
      - **Masque**: 255.255.255.0
      - **Passerelle par défaut**: 10.10.8.1

Attribution par département :

 | Département/Service          | Sous-réseau       | Plage d'adresses utilisables | Masque              | Exemple d'adresses attribuées    |
|------------------------------|-------------------|------------------------------|---------------------|-----------------------------------|
| **Communication**            | `10.10.8.0/26`   | `10.10.8.1 - 10.10.8.62`     | `255.255.255.192`  | Postes : `10.10.8.2-10.10.8.10`  |
| **Développement**            | `10.10.8.64/26`  | `10.10.8.65 - 10.10.8.126`   | `255.255.255.192`  | Postes : `10.10.8.66-10.10.8.80` |
| **Direction**                | `10.10.8.128/27` | `10.10.8.129 - 10.10.8.158`  | `255.255.255.224`  | Postes : `10.10.8.130-10.10.8.135`|
| **Ressources Humaines (RH)** | `10.10.8.160/28` | `10.10.8.161 - 10.10.8.174`  | `255.255.255.240`  | Postes : `10.10.8.162-10.10.8.164`|
| **DSI**                      | `10.10.8.176/28` | `10.10.8.177 - 10.10.8.190`  | `255.255.255.240`  | Postes : `10.10.8.178-10.10.8.180`|
| **Finance et Comptabilité**  | `10.10.8.192/27` | `10.10.8.193 - 10.10.8.222`  | `255.255.255.224`  | Postes : `10.10.8.194-10.10.8.200`|
| **Service Commercial**       | `10.10.8.224/27` | `10.10.8.225 - 10.10.8.254`  | `255.255.255.224`  | Postes : `10.10.8.226-10.10.8.240`|

---
### 6. Les Sauvegardes et les Fichiers
 - **Format des fichiers** : La structure sera sous la forme "NomDépartement_TypeDeDocument_Date" (Exemple : RH_SuiviEmbauches_20250101)
 - **Dossiers de Sauvegarde** : La structure sera sous la forme "Backup-NomDépartement-Fréquence" (Exemple : Backup-DSI-Daily)

---
### 7. Exceptions et Validation
- Toute exception à cette convention doit être validée par le DSI d’EcoTech Solutions.
- Les exceptions doivent être documentées et justifiées pour garantir la traçabilité.
<br>

---
  
## 3) Matériel et dispositifs réseau**
## 4) Adressage
### Réseau global
- **Plage IP** : `10.10.8.0/24`
- **Masque** : `255.255.255.0`
- **Passerelle par défaut** : `10.10.8.1`

### Sous-réseaux par département
Chaque département et service dispose de son propre sous-réseau.

| Département/Service          | Sous-réseau       | Plage d'adresses utilisables | Masque              | Exemple d'adresses attribuées    |
|------------------------------|-------------------|------------------------------|---------------------|-----------------------------------|
| **Communication**            | `10.10.8.0/26`   | `10.10.8.1 - 10.10.8.62`     | `255.255.255.192`  | Postes : `10.10.8.2-10.10.8.10`  |
| **Développement**            | `10.10.8.64/26`  | `10.10.8.65 - 10.10.8.126`   | `255.255.255.192`  | Postes : `10.10.8.66-10.10.8.80` |
| **Direction**                | `10.10.8.128/27` | `10.10.8.129 - 10.10.8.158`  | `255.255.255.224`  | Postes : `10.10.8.130-10.10.8.135`|
| **Ressources Humaines (RH)** | `10.10.8.160/28` | `10.10.8.161 - 10.10.8.174`  | `255.255.255.240`  | Postes : `10.10.8.162-10.10.8.164`|
| **DSI**                      | `10.10.8.176/28` | `10.10.8.177 - 10.10.8.190`  | `255.255.255.240`  | Postes : `10.10.8.178-10.10.8.180`|
| **Finance et Comptabilité**  | `10.10.8.192/27` | `10.10.8.193 - 10.10.8.222`  | `255.255.255.224`  | Postes : `10.10.8.194-10.10.8.200`|
| **Service Commercial**       | `10.10.8.224/27` | `10.10.8.225 - 10.10.8.254`  | `255.255.255.224`  | Postes : `10.10.8.226-10.10.8.240`|

### Sous-réseau pour les serveurs
Un sous-réseau séparé est réservé pour tous les serveurs nécessaires à l'infrastructure, actuel ou futur.

| **Type de Serveurs**         | Sous-réseau       | Plage d'adresses utilisables | Masque              | Exemple d'adresses attribuées    |
|------------------------------|-------------------|------------------------------|---------------------|-----------------------------------|
| **Serveurs**                 | `10.10.9.0/28`   | `10.10.9.1 - 10.10.9.14`     | `255.255.255.240`  | Serveur DNS : `10.10.9.2`<br>Serveur Web : `10.10.9.3`<br>Serveur Fichiers : `10.10.9.4` |

- **Passerelle pour les serveurs** : `10.10.9.1`  

### Répartition des équipements réseau
- **Box FAI (Passerelle principale)** : `10.10.8.1`
- **NAS** : `10.10.8.63`
- **Répéteurs WiFi** : `10.10.8.62`, `10.10.8.126`

---

### Distribution des adresses par département/service

#### 1. **Communication**
- **Sous-réseau** : `10.10.8.0/26`
- **Plage utilisable** : `10.10.8.2 - 10.10.8.62`
- **Attribution** :
  - **Postes** : `10.10.8.2 - 10.10.8.10`
  - **Switch/DHCP** : `10.10.8.61`

#### 2. **Développement**
- **Sous-réseau** : `10.10.8.64/26`
- **Plage utilisable** : `10.10.8.65 - 10.10.8.126`
- **Attribution** :
  - **Postes** : `10.10.8.66 - 10.10.8.80`
  - **Switch/DHCP** : `10.10.8.125`

#### 3. **Direction**
- **Sous-réseau** : `10.10.8.128/27`
- **Plage utilisable** : `10.10.8.129 - 10.10.8.158`
- **Attribution** :
  - **Postes** : `10.10.8.130 - 10.10.8.131`
  - **Switch** : `10.10.8.157`

#### 4. **Ressources Humaines (RH)**
- **Sous-réseau** : `10.10.8.160/28`
- **Plage utilisable** : `10.10.8.161 - 10.10.8.174`
- **Attribution** :
  - **Postes** : `10.10.8.162 - 10.10.8.164`
  - **Switch** : `10.10.8.173`

#### 5. **DSI**
- **Sous-réseau** : `10.10.8.176/28`
- **Plage utilisable** : `10.10.8.177 - 10.10.8.190`
- **Attribution** :
  - **Postes** : `10.10.8.178 - 10.10.8.180`
  - **Switch** : `10.10.8.189`

#### 6. **Finance et Comptabilité**
- **Sous-réseau** : `10.10.8.192/27`
- **Plage utilisable** : `10.10.8.193 - 10.10.8.222`
- **Attribution** :
  - **Postes** : `10.10.8.194 - 10.10.8.201`
  - **Switch** : `10.10.8.221`

#### 7. **Commercial**
- **Sous-réseau** : `10.10.8.224/27`
- **Plage utilisable** : `10.10.8.225 - 10.10.8.254`
- **Attribution** :
  - **Postes** : `10.10.8.226 - 10.10.8.240`
  - **Switch** : `10.10.8.253`

#### 8. **Serveurs**
- **Sous-réseau** : `10.10.9.0/28`
- **Plage utilisable** : `10.10.9.1 - 10.10.9.14`
- **Attribution** :
  - Serveur DNS : `10.10.9.2`
  - Serveur Web : `10.10.9.3`
  - Serveur Fichiers : `10.10.9.4`

---


## 6) Table de Routage
