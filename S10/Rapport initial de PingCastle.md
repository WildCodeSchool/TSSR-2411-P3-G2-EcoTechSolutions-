# 🔍 Rapport initial d’Audit Active Directory – ad.hc.ecotechsolutions.com

## 🏢 Informations Générales

- **Nom du domaine** : `ad.hc.ecotechsolutions.com`
- **Nom NetBIOS** : `AD`
- **Niveau fonctionnel** : Windows Server 2016/2019
- **Contrôleur(s) de domaine** : 1 seul serveur (`SRVWIN01`)
- **Adresse IP DC** : `172.16.10.5`

---

## 👤 Utilisateurs

- **Nombre total d’utilisateurs actifs** : 8
- **Comptes désactivés** : 1
- **Utilisateurs avec mot de passe qui n’expire jamais** : 3 ⚠️
- **Utilisateurs privilégiés** :
  - `Domain Admins` : 2 membres
  - `Enterprise Admins` : vide ✅
  - `Privileged Users` : 1 membre

---

## 🖥️ Contrôleur de Domaine

- **Nom** : `SRVWIN01`
- **OS** : Windows Server 2022
- **Rôles FSMO** : Tous les rôles (RID, PDC, Infrastructure, Schema, DomainNaming) sont sur ce serveur

> ⚠️ Tous les rôles sur un seul DC – Risqué en cas de panne. Pas de redondance.

---

## 🔐 Politiques de Mot de Passe

- **Longueur minimale** : 7 caractères
- **Complexité requise** : Oui
- **Expiration maximale** : 42 jours

> 🔄 Recommandé : longueur ≥ 10, verrouillage compte, historique mots de passe.

---

## 📡 DNS et Réplication

- **Serveur DNS** : SRVWIN01
- **Réplication AD** : Pas d’erreur
- **Ordinateurs AD** : 1 client (`CLIWIN01`)
- **Verrouillages récents de comptes** : Aucun

---

## ⚙️ Recommandations et Axes d'Amélioration

| Domaine                        | Action recommandée |
|-------------------------------|--------------------|
| 🔁 Redondance DC              | Ajouter un second contrôleur de domaine |
| 🔒 Comptes à mot de passe non expirant | Surveiller ou désactiver les comptes concernés |
| 📊 Journalisation              | Implémenter Graylog ou une solution SIEM |
| 📑 GPO                         | Ajouter des GPO de durcissement (session, audit, pare-feu) |
| 👥 Gestion des utilisateurs    | Utiliser GLPI ou scripts PowerShell pour l'automatisation |
| 🧱 Sécurité réseau             | Intégrer pfSense et durcir les flux |
| 🛠 Supervision                 | Mettre en place une supervision via Zabbix |
| 🔐 Sécurité des accès         | Renforcer les mots de passe (12+), envisager le MFA |
| 🗃️ Sauvegardes                | Planifier la sauvegarde régulière de l’AD (NTDS, SYSVOL) |

---

## ✅ Conclusion

Le domaine `ad.hc.ecotechsolutions.com` est bien configuré dans l’ensemble, mais nécessite :
- une meilleure **résilience** (2e DC),
- le **renforcement des mots de passe**,
- l’ajout de **GPOs de sécurité**,
- une **supervision active** et un **SIEM**.

Objectif visé : **Zéro faille détectée** avec PingCastle ou outils similaires ✅
