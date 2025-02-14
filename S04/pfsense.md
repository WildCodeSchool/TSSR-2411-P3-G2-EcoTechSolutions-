# PFSense

Configuration du Firewall

Pré-requis de base du Firewall Pfsense
Une interface réseau Wan :

IP : 10.0.0.4/16
Gateway : 10.0.0.1
Une interface réseau Lan :

IP : 10.10.255.254/16
![Capture d'écran 2025-02-14 094701](https://github.com/user-attachments/assets/8a2c8286-460f-4fba-9531-ec9a1ad081d0)


Configuration du Firewall
1. Ajout d'une interface DMZ
La DMZ (Zone DéMilitarisée) est une zone isolée et séparée du reste du réseau. Son principal objectif est de protéger les données et les systèmes internes d’une entreprise contre les attaques venant de l’extérieur. Le fonctionnement d’une DMZ repose sur une série de règles de sécurité définies par l’entreprise. Ces règles permettent de contrôler le trafic entre le réseau interne, la DMZ et le réseau externe, garantissant ainsi une protection optimale des données et des systèmes internes. La mise en place d’une DMZ présente plusieurs avantages :

* Sécurité renforcée
* Contrôle accru
* Flexibilité
* Performances
  
Pour ce faire, nous avons ajouté une carte réseau dans Proxmox, sur la VM de PfSense, pour la DMZ

![Capture d'écran 2025-02-14 094723](https://github.com/user-attachments/assets/fed988d9-4eab-4b14-8e8d-18542b3b0bb3)



Maintenant, nous pouvons voir qu'elle est bien visible dans la VM


![Capture d'écran 2025-02-14 094839](https://github.com/user-attachments/assets/a9366e4d-ffcd-472e-b17b-0b0267fa7ad8)


Nous devons maintenant l'ajouter sur la page du serveur PfSense en entrant l'adresse https://10.10.255.254 dans un navigateur web, ce qui nous mènera à l'image suivante

![Capture d'écran 2025-02-14 095603](https://github.com/user-attachments/assets/544846cf-57b1-419b-ba86-9eb35947b076)



Une fois à ce stade, nous saisissons les identifiants et accédons à la page de gestion du pare-feu PfSense

![pfsenseconfig1](https://github.com/user-attachments/assets/b688b160-88a7-47d8-9c93-27429ac876f3)



Pour des raisons de sécurité, dans un premier temps, nous allons changer le mot de passe du compte administrateur (Admin). Donc, nous appuyons sur "Change the password in the User Manager" encadré en rouge. Et nous arrivons sur la page pour modifier le mot de passe

![pfsenseconfig2](https://github.com/user-attachments/assets/cd60407b-2a8d-4476-9471-32d9ab7baa08)


Changeons le mot de passe, puis validons et revenons à l'accueil
![pfsenseconfig3](https://github.com/user-attachments/assets/185b00b4-9971-4407-ac73-9461107ef0ab)



Arrivés ici, nous cliquons sur Interfaces puis sur Assignments

![pfsense3](https://github.com/user-attachments/assets/a0beb941-e3a8-4f4f-91ce-962424c90a1b)


Nous cliquons sur OPT1 et nous allons pouvoir configurer cette interface. Dans un premier temps, nous cochons la case Enable interface, puis dans le champ Description, nous saisissons DMZ. Ensuite, dans le menu IPv4 Configuration Type, nous choisissons Static IPv4


![pfsense4](https://github.com/user-attachments/assets/b588e278-f091-4199-ac64-1faacf209b1a)

Plus bas, nous saisissons l'adresse IPv4 statique

![Capture d'écran 2025-02-14 100057](https://github.com/user-attachments/assets/25f6da80-5b1b-4604-b240-d85f05a96852)


Et enfin, nous appliquons les changements

![pfsense6](https://github.com/user-attachments/assets/5c6b09bf-a2ac-4c85-8a4d-fd86c501ac43)


 ## 2. Gestion des ID de VM de groupe
