# S01 USER GUIDE
## 1) Schéma réseau
## 2) Convention de nommage
### **1. Matériel et dispositifs réseau**
| Élément                          | Format de nommage                          | Exemple                          |
|----------------------------------|--------------------------------------------|----------------------------------|
| **Serveurs**                     | `SRV-<OS>-<Rôle>-<Numéro>`                 | `SRV-WIN-AD-01` <br> `SRV-LNX-DNS-01` |
| **Routeurs**                     | `RTR-<Numéro>`                             | `RTR-01`                        |
| **Switchs**                      | `SW-<Numéro>`                              | `SW-01`                         |
| **Points d'accès Wi-Fi**         | `AP-<Numéro>`                              | `AP-01`                         |
| **NAS**                          | `NAS-<Numéro>`                             | `NAS-01`                        |


### **2. Postes de travail**
| Élément                          | Format de nommage                          | Exemple                          |
|----------------------------------|--------------------------------------------|----------------------------------|
| **Postes fixes (PC)**            | `PC-<Service>-<Numéro>`                    | `PC-DEV-01`                     |
| **Postes portables (Laptops)**   | `LT-<Service>-<Numéro>`                    | `LT-COM-01`                     |


### **3. Utilisateurs**
| Élément                          | Format de nommage                          | Exemple                          |
|----------------------------------|--------------------------------------------|----------------------------------|
| **Nom d’utilisateur local**      | `<Prénom>.<Nom>`                           | `jean.dupont`                   |
| **Compte administrateur local**  | `Admin-<Service>`                          | `Admin-DEV`                     |
| **Compte administrateur global** | `AdminEcoTech`                             | `AdminEcoTech`                  |


### **4. Groupes**
| Élément                          | Format de nommage                          | Exemple                          |
|----------------------------------|--------------------------------------------|----------------------------------|
| **Groupes AD par service**       | `G-<Service>`                              | `G-DSI`                         |
| **Groupes AD par projet**        | `G-Proj-<NomProjet>`                       | `G-Proj-NouvelleInfra`          |

### **5. Adresses IP**
| Élément                          | Format de nommage                          | Exemple                          |
|----------------------------------|--------------------------------------------|----------------------------------|
| **Plage IP Serveurs**            | `10.10.9.2` à `10.10.9.14`                 | `10.10.9.2`                     |
| **Plage IP Postes de travail**   | `10.10.8.2` à `10.10.8.62`                 | `10.10.8.2`                   |


### **6. Répertoires et fichiers**
| Élément                          | Format de nommage                          | Exemple                          |
|----------------------------------|--------------------------------------------|----------------------------------|
| **Répertoires projet**           | `Proj_<NomProjet>`                         | `Proj_NouvelleInfra`            |
| **Fichiers de configuration**    | `<Équipement>_Conf_<Date>`                 | `SRV-AD-01_Conf_20250122.txt`   |
| **Sauvegardes**                  | `Backup_<Équipement>_<Date>`               | `Backup_SRV-AD-01_20250122.bak` |

### **7. Autres identifiants**
| Élément                          | Format de nommage                          | Exemple                          |
|----------------------------------|--------------------------------------------|----------------------------------|
| **SSID Wi-Fi**                   | `EcoTech-<Service>`                        | `EcoTech-DEV`                   |
| **Identifiants de VLAN**         | `VLAN-<Service>`                           | `VLAN-COM`                      |


### **8. Cas particuliers**
1. **Départements multi-sites (future évolution)** :
   - Ajouter un identifiant de site : `Bordeaux` devient `BOR`.
   - Exemple pour un serveur : `SRV-WIN-AD-BOR-01`.
2. **Équipements spécifiques (imprimantes, téléphones IP, etc.)** :
   - Ajouter un préfixe en fonction de l'équipement : `PRT-01` pour une imprimante, `TEL-01` pour un téléphone IP.


### **9. Rappel des objectifs**
1. **Uniformité** : Tous les noms respectent une structure claire et identifiable.
2. **Simplicité** : Les noms sont faciles à lire et à retenir.
3. **Scalabilité** : La convention permet d’ajouter de nouveaux services ou équipements sans confusion.


## 3) Adressage
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

## 4) Matériels
## 5) Table de Routage
