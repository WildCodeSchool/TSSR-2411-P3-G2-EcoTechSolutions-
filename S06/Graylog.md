# Installation de Graylog sur Debian

## 0. Prérequis
Avant tout, vous devez faire un Snapshot de votre système.
![snapshot](https://raw.githubusercontent.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/main/Ressources/Images/S06/Graylog/0_Creation_Snapshot.PNG)


## 1. Prérequis
Avant d'installer Graylog, assurez-vous que votre serveur Debian est à jour.

```bash
sudo apt update && sudo apt upgrade -y
```
![Mise à jour](https://raw.githubusercontent.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/main/Ressources/Images/S06/Graylog/1_Mise_%C3%A0_Jour_debian.PNG)



## 2. Installation des dépendances

### 2.1 Installation de Java (OpenJDK 17)

Graylog nécessite Java. Installez OpenJDK 17 avec la commande suivante :

```bash
sudo apt install openjdk-17-jdk -y
```
![Installez OpenJDK 17](https://raw.githubusercontent.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/main/Ressources/Images/S06/Graylog/2_Installation_Java_OpenJDK17.PNG)


Vérifiez l'installation :

```bash
java -version
```
![java -version](https://raw.githubusercontent.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/main/Ressources/Images/S06/Graylog/3_Verification_Installation_Java.PNG)



### 2.2 Installation de MongoDB

Graylog utilise MongoDB pour stocker ses métadonnées.

```bash
sudo apt install -y mongodb-org
```
![Installation de MongoDB](https://raw.githubusercontent.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/main/Ressources/Images/S06/Graylog/4_Installation_MongoDB.PNG)


Démarrez et activez MongoDB au démarrage :

```bash
sudo systemctl enable --now mongod
```
![Démarrez et activez MongoDB au démarrage](https://raw.githubusercontent.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/main/Ressources/Images/S06/Graylog/5_Activation_MongoDB_Demarrage.PNG)


Vérification de la version de  MongoDB  :

```bash
dpkg -l | grep mongodb-org

```
![Vérification Version MongoDB](https://raw.githubusercontent.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/main/Ressources/Images/S06/Graylog/6_Verification_Version_MongoDB.PNG)



### 2.3 Installation d’OpenSearch

Graylog utilise OpenSearch comme moteur de recherche. Installez-le avec les commandes suivantes :

```bash
wget https://artifacts.opensearch.org/releases/bundle/opensearch/2.3.0/opensearch-2.3.0-linux-x64.tar.gz
sudo tar -xzf opensearch-2.3.0-linux-x64.tar.gz -C /opt/
cd /opt/opensearch-2.3.0
```
![Installation OpenSearch](https://raw.githubusercontent.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/main/Ressources/Images/S06/Graylog/7_Installation_OpenSearch.PNG)


Modifiez la configuration `opensearch.yml` :

```yaml
cluster.name: graylog
network.host: 0.0.0.0
discovery.type: single-node
```
![Modification Configuration opensearch.yml](https://raw.githubusercontent.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/main/Ressources/Images/S06/Graylog/8_Modification_Configuration_opensearch.yml.png)


Lancez OpenSearch :

```bash
sudo -u opensearch /opt/opensearch-2.3.0/bin/opensearch
```

Vérifiez qu’OpenSearch fonctionne :

```bash
sudo systemctl status opensearch

```
![Vérification Fonctionnement OpenSearch](https://raw.githubusercontent.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/main/Ressources/Images/S06/Graylog/9_Verification_Fonctionnement_OpenSearch.PNG)


## 3. Installation de Graylog

Ajoutez le dépôt officiel de Graylog :

```bash
wget https://packages.graylog2.org/repo/packages/graylog-6.1-repository_latest.deb
sudo dpkg -i graylog-6.1-repository_latest.deb
sudo apt update && sudo apt install graylog-server -y
```
![Installation Graylog](https://raw.githubusercontent.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/main/Ressources/Images/S06/Graylog/10_Installation_Graylog.PNG)



## 4. Configuration de Graylog

### 4.1 Définition du mot de passe administrateur

Générez un mot de passe sécurisé pour l'utilisateur admin :
(Changez le niveau de difficulté du mot de passe "Azerty1*") 

```bash
echo -n "Azerty1*" | sha256sum
```

Ajoutez la valeur obtenue dans le fichier de configuration :

```bash
sudo nano /etc/graylog/server/server.conf
```

Modifiez les lignes suivantes :

```ini
root_password_sha2 = valeur_du_hash
password_secret = maSuperSecretKey
http_bind_address = 0.0.0.0:9000
```

### 4.2 Démarrage de Graylog

Activez et démarrez le service :

```bash
sudo systemctl enable --now graylog-server
```

Vérifiez l'état :

```bash
sudo systemctl status graylog-server
```
![Démarrage Graylog](https://raw.githubusercontent.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/main/Ressources/Images/S06/Graylog/11_Demarrage_Graylog.PNG)


## 5. Configuration du Pare-feu

Ouvrez les ports nécessaires :

```bash
sudo ufw allow 9000/tcp
sudo ufw allow 9200/tcp
sudo ufw allow 27017/tcp
```
![Configuration Pare-Feu Graylog](https://raw.githubusercontent.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/main/Ressources/Images/S06/Graylog/12_Configuration_PareFeu_Graylog.PNG)


## 6. Accès à l'interface Web

Accédez à Graylog via :

```
http://10.10.8.4:8580
```

Identifiants par défaut :

- **Utilisateur** : `admin`
- **Mot de passe** : celui défini dans `root_password_sha2`

![WUI](https://github.com/WildCodeSchool/TSSR-2411-P3-G2-EcoTechSolutions-/blob/main/Ressources/Images/S06/Graylog/13_WUI.PNG)



