# Mise en place de LAPS


Avant de commencer, téléchargez LAPS gratuitement sur le site de Microsoft. Vous devez télécharger à minima "LAPS.x64.msi" pour les machines 64 bits et "LAPS.x86.msi" pour les machines 32 bits, en fonction de vos besoins. Ici, nous optons pour une version 64 bits.

![Capture d'écran 2025-02-19 205453](https://github.com/user-attachments/assets/26d0e3cd-3cb1-4ebd-ad52-63abb7808f89)

Sur le contrôleur de domaine, nous allons installer les outils de gestion LAPS. Cela pourrait être installé sur un autre serveur où vous avez les outils d'administration Active Directory déjà installés.

Exécutez le package MSI correspondant à la version de Windows de votre serveur : 32 bits ou 64 bits. Vous allez voir, l'installation est simple et s'effectue en quelques clics... Cliquez sur "Next".

![Capture d'écran 2025-02-19 210240](https://github.com/user-attachments/assets/3f82e734-8d2f-4cae-9c4e-33d3adbbc88c)


Cochez l'option "I accept the terms in the License Agreement" et cliquez sur "Next".

![Capture d'écran 2025-02-19 210252](https://github.com/user-attachments/assets/08dd3d95-5856-4016-83ae-6fff81a33293)


Ensuite, vous devez installer tous les outils d'administration (comme sur l'image ci-dessous) et vous pouvez désélectionner l'entrée "AdmPwd GPO Extension" car elle n'est pas utile sur le contrôle de domaine. En fait, le composant "AdmPwd GPO Extension" doit être déployé sur l'ensemble des machines à gérer via LAPS. Poursuivez.

![installation-laps-04](https://github.com/user-attachments/assets/cbc5023f-f653-4eb8-8bfe-996e37d1b447)


Voici l'utilité des différents outils de gestion :

Fat client UI : outil graphique pour la gestion de LAPS
PowerShell module : commandes PowerShell pour LAPS
GPO Editor templates : modèle ADMX de LAPS
Démarrez l'installation, quelques secondes seront suffisantes. Cliquez sur "Finish" une fois que c'est fait.

Ouvrez une console Windows PowerShell sur votre contrôleur de domaine. Il faut que ce soit un contrôleur de domaine en écriture (donc pas un RODC !) et qu'il dispose du rôle FSMO "Maître de schéma" puisque l'on va modifier le schéma Active Directory.

Si vous avez besoin de localiser le contrôleur de domaine qui dispose de ce rôle FSMO, voici une commande PowerShell qui vous donnera la réponse :

![Capture d'écran 2025-02-19 211107](https://github.com/user-attachments/assets/40403bd4-443e-4a83-9a49-6559dd57a100)


Cette modification du schéma Active Directory va ajouter deux attributs au sein des objets de la class "computers" :

ms-MCS-AdmPwd : stocker le mot de passe en clair
ms-MCS-AdmPwdExpirationTime : stocker la date d’expiration du mot de passe
Exécutez la commande suivante pour importer le module PowerShell de LAPS :

![Capture d'écran 2025-02-19 211228](https://github.com/user-attachments/assets/a4e3685e-1e01-4d7c-a6db-e03596ad6f48)


Ensuite, exécuter la commande ci-dessous pour mettre à jour le schéma AD, la commande doit retourner trois lignes avec le statut "Success" à chaque fois.

![Capture d'écran 2025-02-19 211300](https://github.com/user-attachments/assets/4677247b-2f99-4fe8-b286-0333563420de)


Si l'on ouvre la console "Utilisateurs et ordinateurs Active Directory" et que l'on regarde les propriétés d'un ordinateur membre du domaine, on peut voir la présence des deux nouveaux attributs. Voici un exemple :

![Capture d'écran 2025-02-19 211516](https://github.com/user-attachments/assets/f97711b8-0c01-4d0c-a176-516b5e1cd732)


La première étape est faite, passons à la suite.

Les machines qui doivent être managées via LAPS ont besoin de mettre à jour les attributs ms-MCS-AdmPwdExpirationTime et ms-MCS-AdmPwd au sein de notre annuaire Active Directory. Sinon, il ne sera pas possible de stocker dans l'AD la date d'expiration et le mot de passe.

Le module LAPS de PowerShell contient un cmdlet pour réaliser cette action. Pour l'utiliser, c'est tout simple puisqu'il suffit d’indiquer le nom de l’OU cible. Pour ma part, je vais cibler l'OU "PC" (visible sur la copie d'écran ci-dessus) car elle contient les machines que je souhaite gérer avec LAPS. Je vous recommande de préciser le DistinguishedName de l'OU pour être sûr de cibler la bonne OU, sauf si vous êtes sûr qu'il n'y en a qu'une seule qui a ce nom. Pour ce faire nous éxecutons la ligne de commande suivante :
![Capture d'écran 2025-02-21 113325](https://github.com/user-attachments/assets/8dec8433-0163-4269-be91-9d28e4613adc)

### Gérer les permissions par défaut / actuelles


En fonction de la configuration de votre annuaire Active Directory, certains utilisateurs ou groupes ont probablement les permissions pour lire les attributs étendus. Afin d'éviter que les attributs créés par LAPS soient accessibles par n'importe qui, nous devons contrôler et adapter les permissions. Nous devons retirer les permissions aux utilisateurs/groupes qui ont accès aux attributs étendus sur l'OU "PC", s'il y a des entrées correspondantes à des utilisateurs non habilités.
Plutôt que de parcourir les droits via l'interface graphique, on peut s'appuyer sur PowerShell pour visualiser quels sont les comptes qui ont un accès à ces attributs étendus. Pour ma part, je vais auditer l'OU "PC" qui contient mes postes à manager avec LAPS.

 ### Find-AdmPwdExtendedrights -Identity "OU" | Format-Table

Je peux voir que les membres du groupe ont un accès à ces informations : c'est normal, c'est la configuration par défaut.

![Capture d'écran 2025-02-21 114302](https://github.com/user-attachments/assets/27a8a121-4026-421c-918d-d1f125bb5858)


Voyons comment gérer cette permission...

Ouvrez la console "Modification ADSI" puis effectuez un clic droit sur "Modification ADSI" afin de cliquer sur "Connexion". Une fenêtre s'ouvre... Laissez le choix par défaut, à savoir "Contexte d'attribution de noms par défaut" et validez.

![Capture_decran_2025-02-21_103932](https://github.com/user-attachments/assets/9bfee0cf-0e0d-44c9-b5cc-80d4d2653111)



Ensuite, parcourez l'Active Directory jusqu'à trouver l'OU qui contient les ordinateurs managés par LAPS (et donc qui vont venir écrire leur mot de passe). Effectuez un clic droit sur cette OU, pour ma part c'est l'OU "PC" et accédez aux propriétés via un clic droit.

![Capture_decran_2025-02-21_103932](https://github.com/user-attachments/assets/dc8ccaf7-8eaa-4f18-9ff6-28083be9441d)

Cliquez sur l'onglet "Sécurité" puis sur le bouton "Avancé". Ensuite, si l'on souhaite retirer les droits, par exemple sur le groupe "Utilisateurs authentifiés" (même si ici ils n'ont pas les droits, c'est un exemple...), il suffit de sélectionner "Utilisateurs authentifiés" dans la liste et cliquer sur le bouton "Modifier". Il ne reste plus qu'à décocher les droits "Tous les droits étendus".



![Capture_decran_2025-02-21_104124](https://github.com/user-attachments/assets/835e8b69-9b21-4c15-92d5-4808a50f8e16)



 

Sélectionnez dans la liste l’utilisateur ou le groupe qui ne doit pas être capable de visualiser les mots de passe, et retirer l’autorisation « Tous les droits étendus » (si elle est active). Cette permission permet de lire la valeur de l’attribut ms-MCS-AdmPwd et les paramètres confidentiels.
![Capture_decran_2025-02-21_104502](https://github.com/user-attachments/assets/35d5cdfc-8aa3-4b1a-a5a9-cf2107241ce8)


Pour ajouter l'autorisation de lire le mot de passe, nous allons utiliser le cmdlet "Set-AdmPwdReadPasswordPermission" avec deux paramètres qui vont permettre de préciser l'OU (-Identity) et le nom du groupe (ou l'utilisateur) à autoriser (-AllowedPrincipals). Ce qui donne :

Le cmdlet pour cette autorisation se nomme "Set-AdmPwdResetPasswordPermission" et il fonctionne comme le cmdlet précédent. Ce qui donne :


![Capture_decran_2025-02-21_112331](https://github.com/user-attachments/assets/9288b550-4358-4c90-9116-2e4f13fd3bf6)

Si vous utilisez deux groupes, vous pouvez les préciser dans la même commande en les séparant par une virgule. Voici un exemple :


Vous devez répéter cette opération via ces deux commandes sur toutes les OU qui contiennent des machines que vous souhaitez gérer avec LAPS. Attention, ces autorisations s'appliquent sur les sous-OU, donc si je crée une OU sous mon OU "PC", ce sera automatiquement appliqué.

