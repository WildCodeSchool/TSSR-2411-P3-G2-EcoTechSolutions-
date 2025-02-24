```markdown
# Installation de Graylog sur Windows Server 2022

## 1. Prérequis
Avant d'installer Graylog, assurez-vous que votre serveur Windows Server 2022 est à jour.

### 1.1 Installation de Java (OpenJDK 17)
Graylog nécessite Java. Téléchargez et installez OpenJDK 17 :
- Accédez à [https://adoptium.net/](https://adoptium.net/)
- Téléchargez la version OpenJDK 17 LTS pour Windows
- Installez et configurez la variable d'environnement JAVA_HOME :
  - Ouvrez **Paramètres système avancés** → **Variables d’environnement**
  - Ajoutez une nouvelle variable `JAVA_HOME` avec le chemin d’installation de Java

Vérifiez l'installation avec :
```powershell
java -version
```

### 1.2 Installation de MongoDB
Graylog utilise MongoDB pour stocker ses métadonnées.
1. Téléchargez MongoDB depuis [https://www.mongodb.com/try/download/community](https://www.mongodb.com/try/download/community)
2. Installez MongoDB en mode **Standalone**
3. Démarrez le service MongoDB :
   ```powershell
   net start MongoDB
   ```
4. Vérifiez l’installation :
   ```powershell
   mongo --eval "db.version()"
   ```

### 1.3 Installation d’Elasticsearch
Graylog utilise Elasticsearch pour stocker et rechercher les logs.
1. Téléchargez Elasticsearch 7.10.2 depuis [https://www.elastic.co/downloads/past-releases#elasticsearch](https://www.elastic.co/downloads/past-releases#elasticsearch)
2. Extrayez et modifiez `elasticsearch.yml` dans `config/` :
   ```yaml
   cluster.name: graylog
   network.host: 0.0.0.0
   discovery.type: single-node
   ```
3. Démarrez Elasticsearch :
   ```powershell
   .\bin\elasticsearch.bat
   ```

### 1.4 Configuration de Windows Firewall
Ajoutez des règles pour autoriser Elasticsearch, MongoDB et Graylog :
```powershell
New-NetFirewallRule -DisplayName "Elasticsearch" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 9200
New-NetFirewallRule -DisplayName "Graylog Web" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 9000
```

## 2. Installation de Graylog
1. Téléchargez Graylog depuis [https://packages.graylog2.org/](https://packages.graylog2.org/)
2. Extrayez le fichier ZIP
3. Modifiez `graylog.conf` dans le dossier `config/` :
   - Définissez une clé de hachage pour les mots de passe :
     ```powershell
     echo -n "VotreMotDePasse" | sha256sum
     ```
   - Ajoutez cette clé à `password_secret`
   - Définissez `root_password_sha2` avec le hash de votre mot de passe admin
   - Configurez `http_bind_address = 0.0.0.0:9000`

4. Démarrez Graylog :
   ```powershell
   .\bin\graylog-server.bat
   ```

## 3. Accès à l'interface Web
- Ouvrez un navigateur et accédez à :
  ```
  http://<IP-DU-SERVEUR>:9000
  ```
- Connectez-vous avec l'utilisateur `admin` et le mot de passe défini précédemment.

## 4. Configuration des Logs des Serveurs
- Ajoutez des **Inputs** pour recevoir les logs via Syslog ou GELF.
- Configurez les serveurs pour envoyer leurs logs vers Graylog.

Graylog est maintenant installé et fonctionnel sur votre Windows Server 2022 ! 🎉
```

