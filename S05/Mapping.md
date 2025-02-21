# Mappage des lecteurs réseaux pour dossiers partagés

Mise en place de GPO
Ouvrez le Gestionnaire de serveur > Outils > Gestion des Stratégies de Groupe.
![1](https://github.com/user-attachments/assets/97625e55-203c-43e7-93c0-f54b24cb4ff0)


Clique-droit sur "Objets de stratégie de groupe" puis "Nouveau" et nommez l'objet.

![2](https://github.com/user-attachments/assets/4ecb8a76-080e-4fd5-9854-1ce42afaa74d)


Une fois nommée, allez la chercher dans la liste des objets et faites un clique-droit et ensuite "Modifier".

![3](https://github.com/user-attachments/assets/5f93b23b-8f29-48c1-8242-9c49c59eb815)


Dans l'Editeur des stratégies , allez dans Configuration Utilisateur > Preferences > Parametre Windows > Mappages de lecteur. Clique-droit sur "Mappages de lecteur et ensuite "Nouveau > Lecteur mappé".

![4](https://github.com/user-attachments/assets/27331252-fc42-44cd-b7ca-3b62ff53ae92)


Dans les propriétés du lecteur, renseignez le chemin du lecteur. Cochez "Reconnecter". Nommez le libeller. Dans lettre de lecteur, utilisez la lettre du lecteur concerné.

![5](https://github.com/user-attachments/assets/8b1b4390-6bae-4ba4-9ebf-19adda39ff9e)


Dans l'onglet "Commun", cochez "Exécuter dans le contexte de sécurité de l'utilisateur connecté".

![6](https://github.com/user-attachments/assets/de09ec25-9af8-4f16-914f-fb81db54bc3a)


Ensuite, dans l'Editeur des stratégies , allez dans Configuration Utilisateur > Preferences > Parametre Windows > Dossier. Clique-droit sur "Dossier" et ensuite "Nouveau > Dossier".

![7](https://github.com/user-attachments/assets/e4c67994-98ae-49bb-b351-c51ab556d054)


Une fois effectuée, revenez sur le gestionnaire de stratégie de groupe. Clique-droit sur l'Unité d'Organisation (OU) concerné et "lier l'objet de stratégie". Sélectionnez l'objet concerné et validez.

![11](https://github.com/user-attachments/assets/427ee2c6-4ec6-4749-8fe3-e1b8eac66ca9)


Création d'un dossier de partage
Créez un dossier et nommez-le puis clique-droit et allez sur "Propriété".

alt text

Allez dans l'onglet "Partage" puis "Partage avancé"



Cochez "Partager ce dossier", nommez le partage (par default le nom du dossier apparait automatiquement) et cliquez sur "Autorisations".

![Capture d'écran 2025-02-21 095012](https://github.com/user-attachments/assets/9961e99c-7b41-4d52-9d52-fb705c79cd6e)



Ajoutez ou Supprimez les Utilisateurs ou les Groupes D'Utilisateurs autorisés à acceder au dossier. En cliquant sur "Ajouter", une fenetre va s'ouvrir pour ajouter un ou plusieurs utilisateurs.

![Capture d'écran 2025-02-21 095535](https://github.com/user-attachments/assets/c4f0004f-eac5-4deb-9a75-887752bcf62b)


Si vous n'êtes pas sur de l'orthographe, cliquez sur "Avancé" pour affiner votre recherche d'utilisateur.

![Capture d'écran 2025-02-21 095614](https://github.com/user-attachments/assets/ec30557d-a149-43d3-b45c-450ea8ec45b5)


Une fois les utilisateur ajoutés, cochez la case "Modifier" pour chacun des utilisateurs ajoutés puis cliquez sur "Appliquer" et "OK".

![Capture d'écran 2025-02-21 095021](https://github.com/user-attachments/assets/69bfd06f-ae1c-4a60-8539-fd6f09c85e45)


De retour sur cette fenetre, cliquez sur "Appliquer" et "OK".

![Capture d'écran 2025-02-21 095021](https://github.com/user-attachments/assets/f45c2b21-d1d5-4136-bf23-bdcb1ffd2a11)


De retour sur cette fenetre, cliquez sur l'onglet "Sécurité".
Dans l'onglet "Sécurité" cliquez sur "Avancé"
![Capture d'écran 2025-02-21 095457](https://github.com/user-attachments/assets/67c5447b-68d7-4ebe-a161-6099a3a4b1fa)

Cliquez sur "Désactiver l'héritage".

![Capture d'écran 2025-02-21 095406](https://github.com/user-attachments/assets/5a308541-494e-4870-b2f9-8dc0f19b2627)


Validez en cliquant sur " Convertir les autorisations hérités en autorisations explicites sur cet objet", puis sur "OK".

![Capture d'écran 2025-02-21 095427](https://github.com/user-attachments/assets/55c855f6-651a-4bd5-bb74-bd6cff8fc22d)


De retour sur cette page, cliquez sur "Modifier" pour ajouter ou supprimer des utilisateurs qui auront accès au partage.

![Capture d'écran 2025-02-21 095457](https://github.com/user-attachments/assets/d0ac279d-aebe-4e11-ad9b-74a6336cd445)


En cliquant sur "Ajouter", une fenetre va s'ouvrir pour ajouter un ou plusieurs utilisateurs.

![Capture d'écran 2025-02-21 095535](https://github.com/user-attachments/assets/b084f992-6997-4978-bba1-28396c173d25)


Si vous n'êtes pas sur de l'orthographe, cliquez sur "Avancé" pour affiner votre recherche d'utilisateur.

![Capture d'écran 2025-02-21 095614](https://github.com/user-attachments/assets/cfdad5de-53a3-4da2-a7a5-d7cee4846946)


Une fois les utilisateur ajoutés, cochez la case "Contrôle total" pour chacun des utilisateurs ajoutés puis cliquez sur "Appliquer" et "OK".
![Capture d'écran 2025-02-21 095712](https://github.com/user-attachments/assets/f032dbaf-c9fe-4002-82dc-4e9e244452b2)


Maintenant, le dossier partagé est configué pour les utilisateurs ou groupe d'utilisateurs autorisés. Les utilisateurs ou groupe d'utilisateurs non autorisés n'y auront pas accès.
