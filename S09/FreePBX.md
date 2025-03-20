
# VoIP FreePBX

## Sommaire

1. Pré-requis techniques

2. Installation et Configurations des matériels nécessaires

## **1. Pré-requis techniques**

**_FreePBX_** est un logiciel Open Source de VoIP


* IP : 10.10.8.10/16
* Passerelle : 10.10.8.2
* Emplacement dans l'infrastructure : LAN
* Base OS : Linux RedHat
* Installation de FreePBX via l'image .iso officielle

## **2. Installation et Configurations des matériels nécessaires**

## 2. a. Création de la VM

## 2. b. Installation et Configuration de FreePBX

### Installation de FreePBX

1. Démarrez la VM.

2. Sélectionnez `FreePBX 16 Installation (Asterisk 18) - Recommanded` et cliquez sur la touche `Entrée`.

![Capture d'écran 2025-03-20 231247](https://github.com/user-attachments/assets/f2dca89a-4f3e-432e-9ec3-47725d05ec8b)



3. Sélectionnez `Graphical Installation - Output to VGA` et cliquez sur la touche `Entrée`.

![Capture d'écran 2025-03-20 231251](https://github.com/user-attachments/assets/1b0853c1-517a-4f61-8cd1-df78e74e897b)



4. Sélectionnez `FreePBX Standard` et cliquez sur la touche `Entrée`.

![Capture d'écran 2025-03-20 231254](https://github.com/user-attachments/assets/35ac2530-ded7-4d87-aa54-5a5aaccd4a2a)



5. L'installation est automatique

![Capture d'écran 2025-03-20 231258](https://github.com/user-attachments/assets/0a4a4ed2-a13e-4c96-b5c5-7881d8bda29c)



6. Une fois l'installation achevée, il est nécessaire de configurer le mot de passe de Root, cliquez sur `Root Password (Root password is not set)`

![Capture d'écran 2025-03-20 231302](https://github.com/user-attachments/assets/0445bdbe-51bc-48d3-a9b7-27f5981a82bc)



7. Saisissez le mot de passe et confirmez-le (Attention, le clavier est en anglais) et cliquez sur `Done`.

![Capture d'écran 2025-03-20 231306](https://github.com/user-attachments/assets/1aea24db-572f-4bef-b89a-25342bce7721)



8. La vignette `Root Password` a changé en `Root password is set`. Cliquez sur `Finish configuration`.

![Capture d'écran 2025-03-20 231313](https://github.com/user-attachments/assets/cb0749f9-5b38-4d73-94f7-7942066dd186)



9. Une fois la configuration achevé, cliquez sur `Reboot`.

![Capture d'écran 2025-03-20 231317](https://github.com/user-attachments/assets/c59364e9-8a61-4536-82e7-a5f8c290c7bc)


10. Retirez l'image .iso, redémarrez la VM et connectez en tant que `Root`.

![Capture d'écran 2025-03-20 231325](https://github.com/user-attachments/assets/5340c594-472f-4c37-b19b-f0b952c06ad7)



11. Saisissez la commande `localectl`, vous pouvez remarquer que le clavier est toujours en anglais.

```
System Locale: LANG=en_US.UTF-8
    VC Keymap: us
    X11 Layout: us
```

12. Assurez-vous que la langue française est disponible avec la commande `localectl list-locales`, la ligne `fr_FR.utf8` doit être présente.

13. Modifiez la langue du clavier avec les commandes :

```
localectl set-locale LANG=fr_FR.utf8
localectl set-keymap fr
localectl set-x11-keymap fr
```

14. Vérifiez que la langue a bien été prise en compte avec `localectl`. Vous devriez obtenir :

```
System Locale: LANG=fr_FR.UTF-8
    VC Keymap: fr
    X11 Layout: fr
```

15. Ajoutez un utilisateur avec la commande `adduser` et changez son mot de passe avec `passwd`.

16. Changez l'IP du serveur en modifiant le fichier `/etc/sysconfig/network-scripts/ifcfg-eth0`

![Capture d'écran 2025-03-20 231331](https://github.com/user-attachments/assets/f6b81144-faa9-4bbf-834e-8767d3a878be)


17. Modifiez le fichier de configuration SSH avec `nano /etc/ssh/sshd_config` en ajoutant les lignes :

```
PermitRootLogin no
AllowUsers wilder
PasswordAuthentication yes
```


18. Vous pouvez désormais vous connecter via un navigateur web à l'adresse : http://votre-ip, vous accéderez ainsi à l'interface de gestion de FreePBX.

![Capture d'écran 2025-03-20 230614](https://github.com/user-attachments/assets/2f01af2e-ebbf-4ce6-9611-eba900c93867)



### Configuration de FreePBX

1. Remplissez les champs pour vous connecter en tant que `root` puis cliquez sur `Setup System`.

![Capture d'écran 2025-03-20 230619](https://github.com/user-attachments/assets/1ed67b89-f32b-45e7-9ddb-7c4ef2341cc2)


2. Accédez à la partie Administration en cliquant sur `FreePBX Administration` et connectez-vous en `root`.

![Capture d'écran 2025-03-20 230625](https://github.com/user-attachments/assets/0aab7337-f5eb-43ac-9702-d9a2f7c68bab)


3. Cliquez sur `Skip`, l'activation interviendra plus tard.

![Capture d'écran 2025-03-20 230630](https://github.com/user-attachments/assets/01c33be8-4715-4c6b-93ce-e38cb061bfbb)


4. Validez les langues en cliquant sur `Submit`.

![Capture d'écran 2025-03-20 230635](https://github.com/user-attachments/assets/6cd17988-4acf-4124-8a14-161b8eb86fac)


5. Cliquez sur `Abort`.

![Capture d'écran 2025-03-20 230640](https://github.com/user-attachments/assets/8e8d2ee2-69de-4bb0-b555-44265b6f6beb)


6. Cliquez sur `Not Now`. 

![Capture d'écran 2025-03-20 230651](https://github.com/user-attachments/assets/0e88fdba-16da-439f-abfc-9a630b2d8bbe)


7. Vous accédez au Tableau de bord, cliquez sur `Apply Config`.

![Capture d'écran 2025-03-20 230658](https://github.com/user-attachments/assets/16532fd5-78e6-419e-87b5-043681a47b12)


8. Nous allons désormais procéder à l'activation afin de profiter de l'ensemble des fonctionnalités. Allez dans `Admin` puis `System Admin`.

![Capture d'écran 2025-03-20 230703](https://github.com/user-attachments/assets/07cc56f6-4a0a-422b-a5a8-657194dc1441)


9. Le message `This machine is not activated` est affiché, cliquez sur `Activation`.

![Capture d'écran 2025-03-20 230708](https://github.com/user-attachments/assets/0687b075-1c31-4247-afe2-2972d4de37dd)


10. Le message change, cliquez sur `Activate`.

![Capture d'écran 2025-03-20 230713](https://github.com/user-attachments/assets/00863dbf-fc95-4988-a026-f04d0083d51a)


11. Cliquez sur `Activate`.

![Capture d'écran 2025-03-20 230719](https://github.com/user-attachments/assets/3eb9cbfc-a612-4cc9-a7db-293f51dbd9c1)


12. Renseignez le champ `Email Address`, les autres champs vont apparaitre. Remplissez les champs nécessaires.

![Capture d'écran 2025-03-20 230725](https://github.com/user-attachments/assets/9ffb7964-dee7-4175-be36-d67568527f8b)


13. Sélectionnez `I use your products and services with my Business(s) and do not want to resell it` et cochez la cas `No` en bas, puis cliquez sur `Create`.

![Capture d'écran 2025-03-20 230731](https://github.com/user-attachments/assets/abd76cab-e7fd-4120-a907-6423f2921c27)


14. Cliquez sur `Activate`.

![Capture d'écran 2025-03-20 230737](https://github.com/user-attachments/assets/fc931574-31ab-4640-a594-134cdae53de9)


15. Cliquez sur `Skip` sur toutes les pages Partenaire.

16. La fenêtre de mise-à-jour va s'afficher. Cliquez sur `Update Now`

![Capture d'écran 2025-03-20 230741](https://github.com/user-attachments/assets/71fb928d-7bd0-4893-b9b7-5627ee63ed99)


17. Patientez, la mise-à-jour est en cours.

![Capture d'écran 2025-03-20 230746](https://github.com/user-attachments/assets/42b2c7d8-959d-4a80-99a0-db529ff846ea)


18. De retour sur le Tableau de bord, cliquez sur `Apply Config`.

![Capture d'écran 2025-03-20 230750](https://github.com/user-attachments/assets/a8434206-7777-4ee6-9880-791768dc393c)


19. Sur le serveur, saisissez la commande `fwconsole reload --verbose`. Vous pourrez constater les module en erreur. Vous dedvrez réinstaller les modules impactés avec la commande `fwconsole ma install <module>` et les redémarrer avec `fwconsole ma enable <module>`. Retapez la commande `fwconsole reload --verbose` pour vous assurer qu'il n'y a plus d'erreur.

![Capture d'écran 2025-03-20 230757](https://github.com/user-attachments/assets/cce946e7-68a7-4ef2-997b-5ea9ed6568a2)


20. De retour sur la page d'Administration sur le Client, cliquez sur `Aply Config` (si nécessaire), puis allez `Admin` > `Modules Admin` > onglet `Modules Updates`. Sur les lignes en status `Disabled; Pending Upgrade to...`, sélectionnez `Upgrade to ... and Enable`. Cliquez sur `Process`.

![Capture d'écran 2025-03-20 230803](https://github.com/user-attachments/assets/ff184290-d5dc-41cd-9b7a-08fe42998480)


21. Cliquez sur `Confirm`.

![Capture d'écran 2025-03-20 230807](https://github.com/user-attachments/assets/de78d484-bf43-4f3b-ae3b-bf319511eacd)


22. Patientez un moment, puis retournez sur le Tableau de bord et cliquez sur `Apply Config`. Réitérez les étapes **20** à **22** si nécessaire (certains modules peuvent avoir plusieurs mise-à-jour consécutives).

![Capture d'écran 2025-03-20 230812](https://github.com/user-attachments/assets/e031d7a7-90a7-424a-a3d8-a2ce8aa8185e)


23. Si besoin, vous pouvez modifier différents paramètres du serveur depuis le Tableau de bord, `Admin` > `Admin Settings`, comme par exemple le `Hostname`, le `DNS` ou encore le `Network Setting`.

![Capture d'écran 2025-03-20 230817](https://github.com/user-attachments/assets/40105d5f-3322-4c6e-9c40-6557d01a0469)


### Création de postes Client VoIP

1. Pour ajouter des utilisateurs et des lignes, allez dans `Applications` > . `Extensions` (Postes en français).

![Capture d'écran 2025-03-20 230822](https://github.com/user-attachments/assets/732903d4-67d4-455c-b683-6c6400360c46)


2. Rendez-vous dans l'onglet `SIP [chan_pjsip] Extensions` et cliquez sur `Add New SIP [chan_pjsip]`

![Capture d'écran 2025-03-20 230826](https://github.com/user-attachments/assets/808522f6-3d12-4718-a8b4-d367b0a428df)


3. Remplissez les champs `User Extension` (qui correspond au numéro de ligne), `Display Name`, `Secret` et `Password for New User`. Puis cliquez sur `Submit`, puis `Apply Config`.

![Capture d'écran 2025-03-20 230837](https://github.com/user-attachments/assets/43046a43-908c-4100-bd9c-54cbfc3b0b36)


4. Réitérez l'étape 3 pour ajouter les utilisateurs nécessaires.

![Capture d'écran 2025-03-20 230844](https://github.com/user-attachments/assets/869cfd55-e993-48d8-8ea7-050216735ca8)


5. Téléchargez le fichier d'installation de [3CXPhone](https://3cxphone.software.informer.com/download/) disponible avec l'extension .msi et enregistrez le fichier


6. Sur le Client, cliquez sur `Set accounts` puis `New`. Remplissez les champs comme suit puis cliquez sur `OK`.
![Capture d'écran 2025-03-20 230951](https://github.com/user-attachments/assets/daeb115c-3ff3-44e3-974c-5554003397fb)

![Capture d'écran 2025-03-20 231007](https://github.com/user-attachments/assets/c9317769-0569-4ec5-a42a-0873e69da772)

![Capture d'écran 2025-03-20 231011](https://github.com/user-attachments/assets/29e0bdd2-8c79-4052-9ba8-0dfa8c36113f)



7. Pour initier une communication, il faut saisir sur le clavier le numéro de téléphone de votre contact et cliquez sur la touche `Appel`.
![Capture d'écran 2025-03-20 231017](https://github.com/user-attachments/assets/2403a81b-655a-44c0-b1a6-797d34afc438)


![Capture d'écran 2025-03-20 231022](https://github.com/user-attachments/assets/94236e3d-326d-4858-8513-257fe6e60890)



8. Vos communications en VoIP sont opérationnelles.
