![zabbix-logo](./images/zabbix-logo.png)

## ZABBIX installation depuis DEBIAN-12 de l'agent-zabbix.md

Zabbix est une solution de surveillance open source populaire utilisée par les administrateurs système pour surveiller et suivre les performances des serveurs, des réseaux et des applications. Pour utiliser efficacement les capacités de surveillance de Zabbix, vous devez installer l'agent Zabbix sur les machines cibles que vous souhaitez surveiller.

Conditions préalables,

Avant de procéder à l'installation, assurez-vous d'avoir les prérequis suivants :

Connectivité réseau au serveur Zabbix.

Toutes les actions à suivre sont éffectées depuis la session root :

- Étape 1 : Mettre à jour les packages système.

Tout d'abord, nous devons nous assurer que nos packages système sont à jour. 

Ouvrez un terminal ou SSH dans votre système Debian et exécutez la commande suivante :
```
sudo apt update && sudo apt upgrade -y 
```
Cette commande mettra à jour les listes de packages et mettra à niveau tous les packages obsolètes.

- Step 2 : Configuration des dépots Official Repository

```
wget https://repo.zabbix.com/zabbix/7.0/debian/pool/main/z/zabbix-release/zabbix-release_7.0-2+debian12_all.deb
```
```
dpkg -i zabbix-release_7.0-2+debian12_all.deb
```
```
apt update
```
De nouveaux dépots seront installés.
```
nano /etc/apt/sources.list.d/zabbix.list
```
```
# Zabbix main repository
deb https://repo.zabbix.com/zabbix/7.0/debian bookworm main
deb-src https://repo.zabbix.com/zabbix/7.0/debian bookworm main
```
Étape 3 : Installer l'agent Zabbix.

Maintenant que les dépôts ont été configuré, nous pouvons installer l'agent Zabbix en exécutant la commande suivante :

```
apt install zabbix-agent2
```
```
systemctl enable zabbix-agent2.service
```
```
cp /etc/zabbix/zabbix_agent2.conf /etc/zabbix/zabbix_agent2.conf.orig
```

- Étape 4 : Configurer l'agent Zabbix.

Une fois l'installation terminée, nous devons configurer l'agent Zabbix pour communiquer avec le serveur Zabbix.

Ouvrez le fichier de configuration de l'agent Zabbix à l'aide d'un éditeur de texte :

```
nano /etc/zabbix/zabbix_agent2.conf
```
Dans le fichier de configuration, localisez les directives `Server=` et `ServerActive=` et définissez-les sur l'adresse IP ou le nom d'hôte de votre serveur Zabbix.

Les paramètres à modifier sont :
```
Server=zabbix-server-IP
ServerActive=zabbix-server-IP
```
```
# Mandatory: yes
# Default:
# Server=

Server=10.100.80.28
```
```
# Mandatory: no
# Default:
# ServerActive=

ServerActive=10.100.80.28
```
Enregistrez les modifications et quittez l'éditeur de texte.
```
systemctl restart zabbix-agent2
```
```
tail -f /var/log/zabbix/zabbix_agent2.log
```
```
2024/07/12 10:50:28.804413 using plugin 'VFSDir' (built-in) providing following interfaces: exporter
2024/07/12 10:50:28.804416 using plugin 'VfsFs' (built-in) providing following interfaces: exporter
2024/07/12 10:50:28.804419 using plugin 'WebCertificate' (built-in) providing following interfaces: exporter, configurator
2024/07/12 10:50:28.804421 using plugin 'WebPage' (built-in) providing following interfaces: exporter, configurator
2024/07/12 10:50:28.804424 using plugin 'ZabbixAsync' (built-in) providing following interfaces: exporter
2024/07/12 10:50:28.804428 using plugin 'ZabbixStats' (built-in) providing following interfaces: exporter, configurator
2024/07/12 10:50:28.804430 lowering the plugin ZabbixSync capacity to 1 as the configured capacity 1000 exceeds limits
2024/07/12 10:50:28.804433 using plugin 'ZabbixSync' (built-in) providing following interfaces: exporter
2024/07/12 10:50:28.806254 Plugin communication protocol version is 6.4.0
2024/07/12 10:50:28.806271 Zabbix Agent2 hostname: [Zabbix server]
```
Note : Autoriser les ports d’écoute sur le pare-feu

Si vous avez un pare-feu UFW en cours d’exécution sur la machine à surveiller, autorisez les ports nécessaires comme indiqué ci-dessous :
```
sudo ufw allow 10050
sudo ufw reload
```
- Étape 5 : Démarrer et activer l'agent Zabbix.

Pour démarrer l'agent Zabbix, exécutez la commande suivante :
```
sudo systemctl start zabbix-agent
```
Ensuite, activez l'agent Zabbix pour qu'il démarre automatiquement au démarrage du système :
```
sudo systemctl enable zabbix-agent
```
- Étape 6 : Vérifier l'état de l'agent Zabbix.

Pour vous assurer que l'agent Zabbix est en cours d'exécution et communique avec le serveur Zabbix, utilisez la commande suivante :
```
sudo systemctl status zabbix-agent
```
Si l'agent s'exécute correctement, vous devriez voir un message d'état indiquant qu'il est actif et en cours d'exécution.

- Étape 7 : Pour installer zabbix-agent2 sur DEBIAN 12

```
sudo wget https://repo.zabbix.com/zabbix/6.4/debian/pool/main/z/zabbix-release/zabbix-release_6.4-1+debian12_all.deb
```
```
sudo dpkg -i zabbix-release_6.4-1+debian12_all.deb
```
```
sudo apt update
```
```
sudo apt install zabbix-agent2
```
- Étape 8 : Pour installer zabbix-agent2 sur DEBIAN 11
```
sudo wget https://repo.zabbix.com/zabbix/6.4/debian/pool/main/z/zabbix-release/zabbix-release_6.4-1+debian11_all.deb
```
```
sudo dpkg -i zabbix-release_6.4-1+debian11_all.deb
```
```
sudo apt update
```
```
sudo apt install zabbix-agent2
```
Pour démarrer Zabbix-agent2, exécutez la commande suivante :
```
sudo systemctl start zabbix-agent2
```
Ensuite, activez Zabbix-agent2 pour qu'il démarre automatiquement au démarrage du système :
```
sudo systemctl enable zabbix-agent2
```
Une fois l'installation terminée, nous devons configurer Zabbix-agent2 pour communiquer avec le serveur Zabbix.

Ouvrez le fichier de configuration de Zabbix-agent2 /etc/zabbix/zabbix_agent2.conf :

```
sudo nano /etc/zabbix/zabbix_agent2.conf
```
Pour configurer Zabbix-agent2 pour envoyer des métriques au serveur Zabbix, localisez la variable "Server=zabbix-server-IP" & "Server=zabbix-server-IP" :
```
Server=127.0.0.1
```
Saisir l'adresse du serveur Zabbix
```
Server=zabbix-server-IP
```
De plus, accédez à la section Vérifications actives et modifiez la directive pour qu'elle pointe vers l'adresse IP du serveur Zabbix.
```
ServerActive=zabbix-server-IP
```
Assurez-vous également d'ajuster le nom d'hôte du serveur Docker en conséquence. 

Saisir le nom d'hôte de mon serveur Docker.
```
Hostname=name-du-server
```
Enregistrez ensuite les modifications et quittez le fichier de configuration Zabbix.
