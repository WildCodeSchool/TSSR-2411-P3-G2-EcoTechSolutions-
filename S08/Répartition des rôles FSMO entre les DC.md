# Transférer les rôles FSMO via les consoles MMC

## a) Les 5 rôles FSMO

1. **Schema Master**  
2. **Domain Naming Master**  
3. **PDC Emulator**  
4. **RID Master**  
5. **Infrastructure Master**

Initialement, tous les rôles sont généralement sur le **premier DC** créé. Il est conseillé de les **répartir** pour éviter qu’un DC unique ne cumule tous les rôles (et représenterait un point de panne majeur).

## b) Exemple de répartition

- **DC1 : SRV-AD-01 :**  
  - Schema Master  
  - Domain Naming Master  

- **DC2 : SRV-AD-02-TEST **  
  - PDC Emulator  
  - RID Master  

- **DC3 : SRV-AD-03-TEST **  
  - Infrastructure Master  

Cette configuration est courante, mais vous pouvez bien sûr adopter une autre répartition.

## c) Transférer ou “basculer” les rôles FSMO

Cette étape explique précisément comment transférer (« basculer ») les rôles FSMO d'un contrôleur de domaine à un autre via les consoles MMC (interface graphique).

---

### ✅ 1. Préparation préalable

Avant de commencer, vérifiez les points suivants :

- **Connectez-vous directement sur le DC** qui doit recevoir les rôles FSMO.
- Vous devez posséder des **droits administrateurs du domaine** (ou Enterprise Admin pour les rôles Schema Master et Domain Naming Master).

---

### 📌 2. Ouvrir la console MMC appropriée selon le rôle FSMO

Selon le rôle FSMO à transférer, ouvrez la console MMC correspondante :

| **Rôle FSMO**              | **Console MMC à utiliser**                                 |
|----------------------------|------------------------------------------------------------|
| RID Master                 | Utilisateurs et ordinateurs Active Directory               |
| PDC Emulator               | Utilisateurs et ordinateurs Active Directory               |
| Infrastructure Master      | Utilisateurs et ordinateurs Active Directory               |
| Domain Naming Master       | Domaines et approbations Active Directory                  |
| Schema Master              | Schéma Active Directory *(à activer au préalable si besoin)*|

---

### 📌 3. Screenshot des éléments avant et après modification

| ![Début FSMO](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/raw/main/Ressources/Images/S08/R%C3%A9partir%20les%20r%C3%B4les%20FSMO%20entre%20les%20DC/01_Debut_FSMO.PNG) | ![FSMO PDC 1](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/raw/main/Ressources/Images/S08/R%C3%A9partir%20les%20r%C3%B4les%20FSMO%20entre%20les%20DC/02_FSMO_PDC_1.PNG) |
|-------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|
| ![FSMO PDC 2](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/raw/main/Ressources/Images/S08/R%C3%A9partir%20les%20r%C3%B4les%20FSMO%20entre%20les%20DC/02_FSMO_PDC_2.PNG) | ![FSMO RID 1](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/raw/main/Ressources/Images/S08/R%C3%A9partir%20les%20r%C3%B4les%20FSMO%20entre%20les%20DC/03_FSMO_RID_1.PNG) |
| ![FSMO RID 2](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/raw/main/Ressources/Images/S08/R%C3%A9partir%20les%20r%C3%B4les%20FSMO%20entre%20les%20DC/03_FSMO_RID_2.PNG) | ![Fin FSMO](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/raw/main/Ressources/Images/S08/R%C3%A9partir%20les%20r%C3%B4les%20FSMO%20entre%20les%20DC/04_Fin_FSMO.PNG) |


