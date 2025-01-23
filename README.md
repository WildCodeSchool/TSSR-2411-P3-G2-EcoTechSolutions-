# TSSR-2411-P3-G2-EcoTechSolutions-
# Projet de Mise en Place d'une Infrastructure Réseau pour EcoTech Solutions

## 1) Introduction :
Ce projet a été mené par une équipe de cinq personnes : Angel, Benjamin, Adel, Camille et Maxime, dans un environnement virtuel hébergé sur Proxmox. Cet environnement a permis l'installation et la configuration de machines virtuelles et de conteneurs fonctionnant sous divers systèmes d'exploitation.

## 2) L'histoire de la société : 
### Fondation et vision initiale
EcoTech Solutions a été fondée en 2015 à Bordeaux, au cœur d'une région dynamique et engagée dans la transition écologique. Les fondateurs, un groupe de cinq ingénieurs passionnés par les nouvelles technologies et le développement durable, partageaient une vision commune : révolutionner la gestion de l'énergie et des ressources grâce à l'Internet des objets (IoT). Leur objectif était de développer des solutions innovantes permettant de surveiller et de contrôler les systèmes énergétiques pour réduire l'impact environnemental.

### Croissance et partenariats stratégiques
Au fil des années, EcoTech Solutions a connu une croissance rapide grâce à des partenariats stratégiques avec des acteurs clés du secteur énergétique et des institutions publiques. En 2018, l'entreprise a signé un accord avec le gouvernement français pour déployer des réseaux de capteurs intelligents dans les grandes villes, contribuant ainsi à l'amélioration de la gestion des infrastructures et à la réduction des déchets.

### Innovation et réalisations majeures
L'innovation est au cœur de la stratégie d'EcoTech Solutions. Parmi ses réalisations notables, l'entreprise a conçu et mis en œuvre des plateformes de gestion énergétique pour des bâtiments intelligents, des systèmes de surveillance des réseaux d'eau, et des solutions d'optimisation de l'utilisation des énergies renouvelables. Ces projets ont permis de réduire les émissions de carbone de leurs clients de 30 % en moyenne.

### Engagement envers la durabilité
EcoTech Solutions s'engage fermement à promouvoir une économie verte. L'entreprise investit dans la recherche et développement pour créer des technologies toujours plus efficaces et respectueuses de l'environnement. Elle collabore également avec des universités et des centres de recherche pour développer des solutions pionnières en matière de gestion écologique.

### Organisation interne
Aujourd'hui, EcoTech Solutions compte 54 employés permanents répartis dans sept départements principaux :

#### Direction = 2

#### Ressources Humaines = 3

#### Finances et Comptabilité = 8

#### Communication = 9

#### DSI = 3

#### Développement = 14

#### Commercial = 15


Des experts externes collaborent à temps partiel ou ponctuellement avec ces départements, apportant leurs compétences pour répondre à des besoins spécifiques.

### Un futur prometteur
EcoTech Solutions continue d'élargir son influence en France et à l'étranger. Avec un engagement constant envers l'innovation et la durabilité, l'entreprise est bien positionnée pour devenir un leader mondial dans le domaine des solutions IoT pour la gestion énergétique et environnementale.

## 3) Objectis principaux du projet :
### Mettre en place une nouvelle infrastructure réseau :

Concevoir une infrastructure réseau complète et fonctionnelle en réponse aux besoins d'EcoTech Solutions.
Réaliser cette infrastructure sous l’hyperviseur Proxmox fourni.
### Planifier et organiser les sprints de réalisation :

Diviser le projet en sprints d'une semaine.
Identifier les objectifs de chaque sprint et planifier les tâches hebdomadaires de manière précise.
### Créer un schéma réseau prévisionnel :

Définir le matériel réseau nécessaire.
Concevoir les serveurs et attribuer les adresses réseau.
Mettre en place un plan pour assurer la connectivité et la sécurité.
### Etablir une convention de nommage :

Uniformiser les noms des appareils, des utilisateurs, et des ressources réseau pour faciliter la gestion.
### Assurer le suivi des objectifs et tâches :

Utiliser un tableau de suivi (Google Sheet) pour consigner les objectifs hebdomadaires, individuels et collectifs.
Maintenir une transparence sur l’avancée du projet, avec validation régulière par le formateur.
### Documenter le projet :

Rédiger des documentations techniques sur les configurations réalisées.
Fournir un dépôt centralisé avec toutes les sources, exécutables, et documents.

## 4) Sprints, rôles et objectifs :

| Sprint                               | Product Owner (PO) | Scrum Master (SM) | Développeurs               | Objectifs                                                                                                                                     |
|--------------------------------------|--------------------|-------------------|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| Sprint 1                             | Maxime             | Benjamin          | Camille, Angel et Adel    | - Création du schéma réseau prévisionnel, documentation du projet, mise en place d'une convention de nommage.                                 |
| Sprint 2                             | Benjamin           | Camille           | Maxime, Angel et Adel     | - Identification des besoins matériels (serveurs, routeurs, switchs, etc.) et logiciels.                                                      |
| Sprint 3                             | Camille            | Angel             | Maxime, Benjamin et Adel  | - Installation et configuration initiale des serveurs (Windows et Linux).                                                                     |
| Sprint 4                             | Angel              | Adel              | Maxime, Benjamin et Camille| - Configuration réseau de base : VLANs, sous-réseaux et tables de routage.                                                                    |
| Sprint 5                             | Adel               | Maxime            | Benjamin, Angel et Camille| - Mise en place du DHCP, DNS et des configurations IP pour les postes utilisateurs.                                                           |
| Sprint 6                             | Maxime             | Benjamin          | Camille, Angel et Adel    | - Intégration des PC dans un domaine (Active Directory), tests de connectivité et ajustements.                                                |
| Sprint 7                             | Benjamin           | Camille           | Maxime, Angel et Adel     | - Sécurisation de l'infrastructure : gestion des droits utilisateurs, création des groupes, mise en place des sauvegardes.                    |
| Sprint 8                             | Camille            | Angel             | Maxime, Benjamin et Adel  | - Configuration des outils de monitoring (Zabbix, Nagios ou équivalent) pour superviser les performances réseau.                              |
| Sprint 9                             | Angel              | Adel              | Maxime, Benjamin et Camille| - Mise en place d’un serveur de fichiers et d’un système de stockage centralisé (NAS).                                                        |
| Sprint 10                            | Adel               | Maxime            | Benjamin, Angel et Camille| - Finalisation des tests de sécurité réseau (firewalls, segmentation, accès distant sécurisé).                                                |
| Sprint 11                            | Maxime             | Benjamin          | Camille, Angel et Adel    | - Documentation finale (guide utilisateur, configuration réseau et maintenance) et formation des utilisateurs internes.                        |
| Sprint 12                            | Benjamin           | Camille           | Maxime, Angel et Adel     | - Présentation finale du projet (bilan des objectifs atteints, démonstration de l'infrastructure fonctionnelle).                              |



## 5) Choix techniques
### 5.1) Infrastructure réseau
#### a. Matériel réseau
Switchs managés : pour un contrôle précis du réseau et la segmentation via VLAN.
Routeurs :
Matériel dédié (Cisco, MikroTik, Ubiquiti, etc.).
Routeurs logiciels (pfsense, OPNsense, ou VyOS sur des serveurs).
Firewall matériel : pour renforcer la sécurité réseau.
#### b. Réseau local (LAN)
Segmentation par VLAN pour isoler les départements.
Planification des sous-réseaux IP :
Exemple : 10.10.8.0/24 -> Subnetting pour les VLANs.
Réservation d'adresses IP fixes pour les serveurs et équipements critiques.
Activation de Spanning Tree Protocol (STP) pour éviter les boucles réseau.
#### c. Sécurité réseau
Pare-feu matériel/logiciel : pour contrôler les flux entrants/sortants.
VPN : pour une gestion sécurisée à distance des équipements réseau.
### 5.2 Infrastructure serveur
#### a. Serveurs physiques ou virtuels
Utilisation de l'hyperviseur Proxmox pour déployer :
Contrôleurs de domaine (Windows Server).
Serveurs de fichiers (Samba, Nextcloud).
Serveurs de sauvegarde (Bareos, Veeam).
#### b. Services réseau essentiels
Active Directory (AD) :
Gestion centralisée des utilisateurs et groupes.
Application de politiques de sécurité via GPO.
DHCP : gestion des adresses IP dynamiques.
DNS interne : résolution des noms locaux et amélioration des performances réseau.
#### c. Stockage
Remplacement du NAS grand public par :
Un NAS professionnel (Synology, QNAP) avec RAID pour redondance.
###  5.3 Sécurité
#### a. Authentification
Passage des PC en domaine (via Active Directory).
Authentification forte avec :
Utilisation de mots de passe complexes et changement régulier.
Intégration d’une authentification multi-facteurs (MFA).
#### b. Protection des données
Sauvegardes régulières avec :
Stratégies de rétention (quotidienne, hebdomadaire, mensuelle).
Stockage des sauvegardes hors site ou dans le cloud.
Mise en place de politiques de confidentialité des données.
###  5.4 Communication
#### a. Messagerie
Migration vers une solution sécurisée si nécessaire (Microsoft 365, Google Workspace).
### 5.5 Nomadisme et mobilité
Préparer une infrastructure prête pour le télétravail :
VPN sécurisé pour les connexions à distance.
Portails d’accès distant avec authentification forte.
Ordinateurs équipés pour un usage mixte bureau/domicile.
### 5.6 Outils de suivi et documentation.
Stockage de la documentation dans un dépôt GitHub ou un serveur interne (Wiki).


## 6) Difficultés / Solutions
## 7) Suggestions d'amélioration
## 8) Conclusion
