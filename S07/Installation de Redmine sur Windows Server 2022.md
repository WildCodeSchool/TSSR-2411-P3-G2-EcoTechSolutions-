
Installation de Redmine sur Windows Server 2022

## Prérequis
Avant de commencer l'installation, vous devez avoir les éléments suivants :

- **Ruby** installé sur votre serveur.
- **MariaDB** installé et configuré.
- **Redmine** téléchargé et configuré.

### Étape 1 : Installation de Ruby
1. Téléchargez la dernière version de **Ruby** à partir du site officiel ou utilisez l'installateur Ruby.
2. Lors de l'installation, acceptez les termes de la licence et sélectionnez les composants que vous souhaitez installer.

   **Images :**
   - ![Installation de Ruby](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S07/Redmine/01_Installation_Ruby.PNG)
   - ![Installation de Ruby (Suite)](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S07/Redmine/02_Installation_Ruby2.PNG)
   - ![Installation de Ruby (Final)](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S07/Redmine/03_Installation_Ruby3.PNG)

### Étape 2 : Installation de MariaDB
1. Téléchargez et installez **MariaDB** sur votre serveur Windows.
2. Une fois l'installation terminée, assurez-vous que le service MariaDB est démarré.

   **Image :**
   - ![Démarrage du service MariaDB](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S07/Redmine/04_Installation_MariaDB.PNG)

### Étape 3 : Téléchargement et Configuration de Redmine
1. Téléchargez la dernière version de **Redmine** depuis le site officiel et extrayez les fichiers dans un dossier de votre choix (par exemple `C:\Redmine`).
2. Allez dans le dossier de **Redmine** via la ligne de commande et commencez la configuration.

   **Image :**
   - ![Installation de Redmine](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S07/Redmine/05_Installation_Redmine.PNG)

### Étape 4 : Configuration du fichier `database.yml`
1. Ouvrez le fichier `config/database.yml` de Redmine dans un éditeur de texte.
2. Modifiez les informations de connexion à la base de données pour utiliser **MariaDB**. Utilisez les paramètres suivants :
   ```yaml
   production:
     adapter: mysql2
     database: redmine
     host: localhost
     username: root
     password: "Azerty1*"
     encoding: utf8mb4
     transaction_isolation: "READ-COMMITTED"
  ![Modification du fichier `database.yml`](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S07/Redmine/06_Modification_Database_YML.PNG)

  ### Étape 5 : Installation des dépendances Ruby (Gems)
1. Ouvrez PowerShell dans le dossier `C:\Redmine`.
2. Exécutez les commandes suivantes pour installer les dépendances :
   ```bash
   gem install bundler
   bundle install --without development test
  ![Installation des Gems](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S07/Redmine/07_Installation_Gems_Dependances.PNG)


### Étape 6 : Démarrage de Redmine en mode production
1. Une fois les dépendances installées, lancez Redmine en utilisant la commande suivante :
   ```bash
   bundle exec rails server -e production
  ![Redmine en mode Production](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S07/Redmine/08_Redmine_Production1.PNG)


  ### Étape 7 : Accès à Redmine depuis le navigateur
1. Ouvrez un navigateur web et accédez à l'adresse suivante pour voir Redmine en fonctionnement :
- ![Page d'accueil de Redmine](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S07/Redmine/09_Redmine.PNG)



