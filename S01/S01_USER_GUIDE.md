# S01 USER GUIDE
## 1) Schéma réseau
## 2) Convention de nommage
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
- **Serveurs réseau (sous-réseau dédié)** : `10.10.9.0/28`
  - Exemple :
    - Serveur de domaine (AD/DNS) : `10.10.9.2`
    - Serveur web (HTTP/HTTPS) : `10.10.9.3`
    - Serveur de fichiers (NAS avancé) : `10.10.9.4`

---

Avec cette structure, le réseau est bien segmenté, et l'ajout d'autres serveurs est simple grâce à la réserve disponible dans le sous-réseau dédié.


## 4) Matériels
## 5) Table de Routage
