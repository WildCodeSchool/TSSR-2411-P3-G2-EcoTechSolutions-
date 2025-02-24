# Installation de Graylog sur Debian

## 1. Prérequis

Avant d'installer Graylog, assurez-vous que votre serveur Debian est à jour.

```bash
sudo apt update && sudo apt upgrade -y
```

## 2. Installation des dépendances

### 2.1 Installation de Java (OpenJDK 17)

Graylog nécessite Java. Installez OpenJDK 17 avec la commande suivante :

```bash
sudo apt install openjdk-17-jdk -y
```

Vérifiez l'installation :

```bash
java -version
```

### 2.2 Installation de MongoDB

Graylog utilise MongoDB pour stocker ses métadonnées.

```bash
sudo apt install -y mongodb-org
```

Démarrez et activez MongoDB au démarrage :

```bash
sudo systemctl enable --now mongod
```

Vérification de la version de  MongoDB  :

```bash
dpkg -l | grep mongodb-org

```

### 2.3 Installation d’OpenSearch

Graylog utilise OpenSearch comme moteur de recherche. Installez-le avec les commandes suivantes :

```bash
wget https://artifacts.opensearch.org/releases/bundle/opensearch/2.3.0/opensearch-2.3.0-linux-x64.tar.gz
sudo tar -xzf opensearch-2.3.0-linux-x64.tar.gz -C /opt/
cd /opt/opensearch-2.3.0
```

Modifiez la configuration `opensearch.yml` :

```yaml
cluster.name: graylog
network.host: 0.0.0.0
discovery.type: single-node
```

Lancez OpenSearch :

```bash
sudo -u opensearch /opt/opensearch-2.3.0/bin/opensearch
```



Vérifiez qu’OpenSearch fonctionne :

```bash
sudo systemctl status opensearch

```

## 3. Installation de Graylog

Ajoutez le dépôt officiel de Graylog :

```bash
wget https://packages.graylog2.org/repo/packages/graylog-6.1-repository_latest.deb
sudo dpkg -i graylog-6.1-repository_latest.deb
sudo apt update && sudo apt install graylog-server -y
```

## 4. Configuration de Graylog

### 4.1 Définition du mot de passe administrateur

Générez un mot de passe sécurisé pour l'utilisateur admin :

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
http_bind_address = 127.0.0.1:9000
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

## 5. Configuration du Pare-feu

Ouvrez les ports nécessaires :

```bash
sudo ufw allow 9000/tcp
sudo ufw allow 9200/tcp
sudo ufw allow 27017/tcp
```

## 6. Accès à l'interface Web

Accédez à Graylog via :

```
http://<IP_SERVEUR>:9000
```

Identifiants par défaut :

- **Utilisateur** : `admin`
- **Mot de passe** : celui défini dans `root_password_sha2`

## Conclusion

Graylog est maintenant installé sur Debian 🎉! Vous pouvez commencer à configurer vos sources de logs.

