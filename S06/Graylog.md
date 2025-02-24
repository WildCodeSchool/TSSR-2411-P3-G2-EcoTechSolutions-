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

### 2 Installation de Docker et Graylog sur Windows Server 2022

##  Étape 1 : Installation de Docker

###  1.1 Activer les fonctionnalités requises  
Dans **PowerShell (Admin)**, exécute :

```powershell
Install-WindowsFeature -Name Containers -IncludeAllSubFeature -Restart
```

Après redémarrage, active **Hyper-V** :

```powershell
Install-WindowsFeature -Name Hyper-V -IncludeManagementTools -Restart
```

⚠ **Assure-toi que la virtualisation est activée dans le BIOS**.

---

###  1.2 Installer Docker  
1. **Télécharge Docker Desktop** 👉 [Lien officiel](https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe)  
2. **Lance l’installation** et coche **"Use WSL 2 instead of Hyper-V"** si disponible.  
3. **Ajoute Docker au `Path`** (si nécessaire) :

```powershell
[System.Environment]::SetEnvironmentVariable("Path", $Env:Path + ";C:\Program Files\Docker\Docker", [System.EnvironmentVariableTarget]::Machine)
```

---

###  1.3 Vérifier Docker  
Vérifie que Docker fonctionne :

```powershell
docker version
```

Si tout est correct, tu verras la version de **Docker Engine** et **Docker Client**.

---

### Installation et configuration de Graylog avec Docker

###  2.1 Créer un fichier `docker-compose.yml`  
Dans un dossier, crée un fichier `docker-compose.yml` et ajoute :

```yaml
version: '3.8'
services:
  mongo:
    image: mongo:6.0
    container_name: mongo
    restart: always
    volumes:
      - mongo_data:/data/db

  opensearch:
    image: opensearchproject/opensearch:2.3.0
    container_name: opensearch
    environment:
      - discovery.type=single-node
      - "OPENSEARCH_JAVA_OPTS=-Xms512m -Xmx512m"
      - "DISABLE_INSTALL_DEMO_CONFIG=true"
      - "DISABLE_SECURITY_PLUGIN=true"
    volumes:
      - os_data:/usr/share/opensearch/data
    ulimits:
      memlock:
        soft: -1
        hard: -1
    ports:
      - "9200:9200"

  graylog:
    image: graylog/graylog:6.1
    container_name: graylog
    restart: always
    environment:
      - GRAYLOG_PASSWORD_SECRET=super-secret-key
      - GRAYLOG_ROOT_PASSWORD_SHA2=hashed-password
      - GRAYLOG_HTTP_EXTERNAL_URI=http://127.0.0.1:9000/
    depends_on:
      - mongo
      - opensearch
    ports:
      - "9000:9000"
      - "1514:1514"
      - "12201:12201"

volumes:
  mongo_data:
  os_data:
```

📌 **Remarque :**  
- Remplace `hashed-password` par un mot de passe SHA2 généré avec :  
  ```powershell
  echo -n "tonMotDePasse" | openssl sha256
  ```
- Change `super-secret-key` par une clé sécurisée.

---

###  2.2 Démarrer Graylog

Dans le dossier où se trouve `docker-compose.yml`, exécute :

```powershell
docker-compose up -d
```

📌 **Explication** :  
- `-d` = Exécuter en arrière-plan.  
- Cette commande va télécharger et démarrer **MongoDB, OpenSearch et Graylog**.

---

###  2.3 Vérifier les conteneurs  
Exécute :

```powershell
docker ps
```

Tu devrais voir `mongo`, `opensearch` et `graylog` en cours d’exécution.

---

###  2.4 Accéder à Graylog  
Ouvre un navigateur et va sur :  
👉 **http://127.0.0.1:9000**  

🔑 **Identifiants par défaut** :
- **Utilisateur** : `admin`
- **Mot de passe** : celui défini dans `GRAYLOG_ROOT_PASSWORD_SHA2`

---

##  Conclusion  
Graylog est maintenant installé sur **Windows Server 2022** avec **Docker** ! 🚀  
Besoin d’aide pour configurer des sources de logs ? 😊


```

