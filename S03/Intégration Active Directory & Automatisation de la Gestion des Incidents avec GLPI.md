# 🚀 Intégration Active Directory & Automatisation de la Gestion des Incidents avec GLPI

## 📌 Objectifs

1. **Synchronisation Active Directory (AD) avec GLPI**
2. **Mise en place de la gestion des incidents et du ticketing**
3. **Développement d'un script PowerShell pour automatiser la création de tickets**

---

## 1️⃣ Synchronisation AD avec GLPI

✅ **GLPI installé et configuré** sur **Debian 12**.  
✅ **AD en place** sur **Windows Server 2022** avec les OUs suivantes :
   - Bordeaux
   - Nantes
   - Paris

### 🔹 À faire :
- Configurer **la synchronisation AD-GLPI** pour importer utilisateurs, groupes et ordinateurs.
- Vérifier que **les unités organisationnelles (OU)** sont bien prises en compte.
- Automatiser la synchronisation pour éviter une gestion manuelle.

## 1️⃣.1️⃣ Voici le script powershell pour la synchronisation de L'AD avec GLPI : 

Lorem ipsum
---

## 2️⃣ Gestion des incidents et ticketing

✅ **GLPI intègre déjà un module de gestion des tickets**.
✅ **Installation du plugin FusionInventory** *(optionnel mais recommandé)* pour l’inventaire automatique des machines et logiciels.

### 🔹 Types d'incidents à gérer :
- **Problèmes utilisateurs** :
  - Mot de passe oublié
  - Compte bloqué
  - Problème d’accès
- **Problèmes matériels** :
  - Panne PC
  - Imprimante HS
  - Disque dur plein
- **Problèmes réseau** :
  - Connexion lente
  - Accès refusé
  - VPN KO
- **Demandes spécifiques** :
  - Création de compte
  - Ajout à un groupe AD

---

## 3️⃣ Développement du script PowerShell sur Windows Server 2022

📌 **Objectif : Exécuter un script PowerShell sur Windows Server interagissant avec GLPI.**

### 🔹 Actions prévues :
- **Création automatique d’un ticket GLPI** via API REST *(ex : un compte bloqué en AD crée un ticket automatiquement)*.
- **Recherche d’un ticket existant** pour éviter les doublons.
- **Ajout automatique de logs et d’infos systèmes** dans le ticket.
- **Possibilité d’escalader un ticket** vers un service spécifique *(DSI, Exploitation...)*.
- **(Optionnel) Envoi d’une alerte mail** au support lors de la création d’un ticket.

---

## 📌 Prochaine étape : Développement du script PowerShell
✅ Début du codage du script PowerShell pour la gestion automatique des tickets GLPI 🎯.
