# 🎯 Tutoriel PingCastle – Audit Active Directory et Objectif 0 %

## 🔧 1. Installation de PingCastle

### 📥 Télécharger PingCastle
1. Rendez-vous sur [https://www.pingcastle.com/download/](https://www.pingcastle.com/download/)
2. Cliquez sur **"Download PingCastle"**
3. Téléchargez le fichier `.zip` (ex : `PingCastle_2.X.X.zip`)
4. Extrayez le contenu dans un dossier (ex : `C:\Outils\PingCastle`)

> 📁 Il n'y a **pas d'installation à proprement parler**, c'est un exécutable autonome.

---

## 🧪 2. Audit de l'Active Directory

### 🖥️ Exécuter PingCastle
1. **Ouvrir PowerShell** ou **cmd** en mode **administrateur**
2. Se placer dans le dossier PingCastle :
   ```powershell
   cd C:\Outils\PingCastle
   ```
3. Lancer l’analyse du domaine :
   ```cmd
   PingCastle.exe --healthcheck
   ```

> 🔐 Utilisez un **compte ayant des droits de lecture sur le domaine** (ex : administrateur du domaine)

4. Entrez le nom du domaine si demandé (ex : `ecotechsolutions.local`)
5. Laissez PingCastle analyser… (~1-3 minutes)
6. Un rapport HTML sera généré dans le dossier, ex :
   ```
   AD_ecotechsolutions.local_HealthCheck_2025-03-28-14-22.html
   ```

---

## 📊 3. Analyse des Résultats

### ✅ Ouvrir le rapport HTML
- Double-cliquez sur le fichier HTML généré.
- Vous verrez une **vue d’ensemble avec un score de sécurité (%)** :  
  - **0 % = Parfait**
  - >50 % = Risque élevé ⚠️

### Les principales sections :
| Section | Signification | Exemple |
|--------|----------------|---------|
| **Stale Objects** | Objets (comptes, ordinateurs) non utilisés | PC éteint depuis 90+ jours |
| **Delegation** | Délégations AD mal configurées | Privilèges excessifs |
| **Privileged Accounts** | Trop d’utilisateurs dans "Domain Admins" | ex : Utilisateur temporaire dans Admins |
| **Password Policy** | Mots de passe faibles ou non expirés | Absence de complexité |
| **Trusts** | Relations inter-domaines risquées | Domaine externe inconnu |
| **Kerberos** | Vulnérabilités liées à Kerberos | Compte avec SPN exposé |

> 📌 **Note le score initial affiché** (ex : 47 %) – utile pour mesurer la progression après correctifs.

---

## 🛠️ 4. Effectuer les Actions Correctives

### 🔐 Sécurité des mots de passe
- Appliquer une **GPO stricte**
  - Complexité, longueur (12+), expiration tous les 90 jours

### 👥 Comptes à privilèges
- Vérifie et **réduis les membres de "Domain Admins"**
  - Supprime tout compte inutile ou temporaire

### 🧓 Comptes obsolètes
- **Désactive ou supprime** les comptes utilisateurs/PC inactifs depuis >90 jours :
   ```powershell
   Search-ADAccount -AccountInactive -TimeSpan 90.00:00:00
   ```

### 🧱 Gérer les délégations
- Vérifie les délégations dans les OUs via la console ADUC (clic droit → délégation de contrôle)

### 🔐 LAPS
- Met en place LAPS pour la gestion automatique des mots de passe locaux

### 🧪 Restreindre SPN et Kerberos
- Lance la commande suivante :
   ```cmd
   PingCastle.exe --riskassessment
   ```
   Cela t’indiquera les failles liées à Kerberos (ex: "Kerberoasting")

### 🧹 Nettoyage DNS, SIDHistory, Trusts
- Supprime les **entrées DNS obsolètes**
- Vérifie les **relations d’approbation** dans "Domaines et approbations AD"
- Nettoie les attributs **SIDHistory** inutiles

---

## 🔁 5. Repasser PingCastle

1. Refaire l’analyse :
   ```cmd
   PingCastle.exe --healthcheck
   ```
2. Comparer le **nouveau score** avec le précédent.
3. Répéter jusqu’à **atteindre 0 %** (ou s’en approcher, <5 % est très bien).

---

## 📌 Résumé : Objectif 0 %

| Étape | Action | Objectif |
|-------|--------|----------|
| 🔍 Audit initial | `PingCastle.exe --healthcheck` | Obtenir le score de départ |
| 🛠 Correctifs | GPO, LAPS, AD Cleanup, Droits | Réduire les failles |
| 🔁 Nouvelle analyse | Relancer PingCastle | Suivre l’évolution |
| 🏁 Final | Objectif : 0 % ou le plus bas possible | ✅ |
