# Documentation Création domaine Active Directory EcoTechSolution

1. [L'AD Windows Server 2022 GUI avec les rôles AD-DS, DHCP, DNS](#server)
2. [L'AD Windows Core 2022 Core avec le rôle AD-DS](#core)
3. [Créer une Unité d'organisation sur AD](#ou)

### 1- L'AD Windows Server GUI
<span id="server"></span>

<br>

---

1.1 Installation des rôles :

  - Ouvrir Gestionnaire de serveur.

  - Aller dans **Gérer** > Ajouter des **rôles et fonctionnalités**.

  - Sélectionner **Installation basée sur un rôle ou une fonctionnalité** et cliquer sur Suivant.

  - Sélectionner le serveur que vous avez crée et cliquer sur Suivant.

  - Cocher **Services de domaine Active Directory** (AD-DS), **Serveur DNS**, et **Serveur DHCP.**

  - Ajouter les fonctionnalités requises lorsqu'elles sont demandées.

  - Cliquer sur Suivant jusqu'à l'installation et attendre la fin.

<br>

1.2 Promotion en contrôleur de domaine

  - Dans **Gestionnaire de serveur**, cliquer sur la notification indiquant Configuration requise pour AD-DS.

  - Sélectionner **Promouvoir ce serveur en contrôleur de domaine**.

  - Choisir Ajouter une nouvelle forêt, entrer un nom pour votre forêt par exemple corp.EcoTechSolution.com comme nom de domaine racine, et cliquer sur Suivant.

  - Définir un mot de passe pour le mode de récupération des services d'annuaire (DSRM) et cliquer sur Suivant.

  - Laisser les paramètres par défaut pour DNS et cliquer sur Suivant.

  - Vérifier le nom NetBIOS et cliquer sur Suivant.

  - Vérifier les chemins d'installation, puis cliquer sur Suivant.

  - Vérifier la configuration et cliquer sur Installer.

  - Le serveur redémarrera automatiquement après l'installation.

<br>

1.3 Configuration DHCP

  - Ouvrir **Gestionnaire DHCP**.

  - Sélectionner votre serveur, puis **IPv4**.

  - Cliquer sur **Nouvelle étendue** et suivre l'assistant :

  - Nom : (Entrer le nom de votre étendue)

  - Adresse de début : rentrer votre adresse IP. Par exemple : 192.168.1.100 (Les adresses suivantes sont à modifier selon vos préférences)

  - Adresse de fin : Par exemple : 192.168.1.200

  - Masque de sous-réseau :  par exemple : 255.255.255.0

  - Passerelle : Par exemple  192.168.1.1

  - Serveur DNS : Par exemple :192.168.1.10
  - Activer l'étendue et autoriser DHCP dans Active Directory.

<br>

---

### 2 - L'AD Windows Core
<span id="core"></span>

2.1 Installation du rôle AD-DS

  - Ouvrir sconfig en tapant sconfig dans la console.

  - Sélectionner 6 pour mettre à jour le serveur.

  - Configurer l'adresse IP et le nom d'hôte.

  - Ouvrir **Gestionnaire de serveur** sur votre serveur GUI crée précédement.

  - Ajouter votre serveur à la gestion à distance.

  - Depuis Gestionnaire de serveur, aller dans **Ajouter des rôles et fonctionnalités**.

  - Sélectionner **Installation basée sur un rôle ou une fonctionnalité**.

  - Sélectionner votre serveur core et ajouter **Services de domaine Active Directory** (AD-DS).

  - Attendre l'installation complète.

<br>

2.2 Promotion en contrôleur de domaine

  - Depuis votre serveur créer précédement, ouvrir Gestionnaire de serveur.

  - Aller dans Outils > Utilisateurs et ordinateurs Active Directory.

  - Créer un nouvel ordinateur avec votre serveur dans Computers.

  - Depuis votre serveur, joindre le domaine en passant par sconfig :

  - Sélectionner 1 (Changer le nom de l'ordinateur).

  - Sélectionner 2 (Joindre un domaine).

  - Entrer un nom, pas exemple corp.EcoTechSolution.com et les informations d'authentification.

  - Redémarrer le serveur.

  - Depuis votre serveur, ouvrir Gestionnaire de serveur, aller dans AD-DS, et ajouter votre serveur core comme Contrôleur de domaine supplémentaire.

  - Sélectionner Ajouter un contrôleur de domaine à un domaine existant.

  - Laisser les options par défaut et terminer l'assistant.

  - Attendre le redémarrage automatique.

<br>

3. Configuration de la réplication entre les DC

3.1 Vérification de la réplication

  - Ouvrir **Outils > Sites et services Active Directory**.

  - Déployer **Sites > Default-First-Site-Name > Servers.**

  - Vérifier que votre serveur GUI et votre serveur core créer dans les etapes précédentes et apparaissent.

  - Vérifier les connexions de réplication sous NTDS Settings.

<br>

3.2 Forcer une synchronisation

  - Depuis **Sites et services Active Directory**, cliquer droit sur votre serveur en GUI > Tout répliquer.

  - Répéter l'opération sur votre serveur core.

<br>

3.3 Vérification de l'état de la réplication

  - Ouvrir Invite de commandes et exécuter :

*repadmin /showrepl*

*repadmin /replsummary*

*Get-ADReplicationPartnerMetadata -Target "corp.EcoTechSolution.com" -Partition*

  - Vérifier que toutes les connexions sont réussies.

<br>

4. Vérifications finales

4.1 Vérification des services AD, DNS et DHCP

  - Ouvrir Services.msc sur vos serveur GUI et core.

  - Vérifier que les services ADWS, DNS, DHCP Server sont en état En cours d'exécution.

  <br>

  - La configuration du domaine créer précédement (Par exemple on avait créer corpEcoTechSolution.com plus haut) avec deux contrôleurs de domaine en réplication complète est terminée.

---
### 3 - Création d'une OU via la console Active Directory Users and Computers
<span id="ou"></span>


1. **Ouvrir la console** :
   - Connectez-vous au serveur Active Directory.
   - Ouvrez la console "Active Directory Users and Computers" (ADUC) en exécutant `dsa.msc` via "Exécuter" (`Win + R`).

  <br>

2. **Créer l'OU** :
   - Naviguez vers le domaine où vous souhaitez créer l'OU.
   - Faites un clic droit sur le domaine ou un conteneur existant.
   - Sélectionnez **Nouveau** > **Unité d'organisation**.
   - Entrez le nom de l'OU et validez avec **OK**.

 <br>

### Création d'une OU via PowerShell

Vous pouvez également créer une OU avec PowerShell :

```powershell
New-ADOrganizationalUnit -Name "NomDeVotreOU" -Path "DC=example,DC=com" -ProtectedFromAccidentalDeletion $true
```

Explication des paramètres :
- `-Name "NomDeVotreOU"` : Nom de l'OU à créer.
- `-Path "DC=example,DC=com"` : Emplacement où l'OU sera créée.
- `-ProtectedFromAccidentalDeletion $true` : Active la protection contre la suppression accidentelle.

 <br>
 
## Vérification
Après la création, vérifiez que l'OU apparaît bien dans Active Directory en rafraîchissant la console ADUC ou en exécutant la commande suivante dans PowerShell :

```powershell
Get-ADOrganizationalUnit -Filter "Name -eq 'NomDeVotreOU'"
```


---

