![zabbix-logo](./images/zabbix-logo.png)

## ZABBIX installation depuis DEBIAN-12 de l'agent Zabbix 'zabbix-agent2'.

Zabbix est une solution de surveillance open source populaire utilisée par les administrateurs système pour surveiller et suivre les performances des serveurs, des réseaux et des applications. Pour utiliser efficacement les capacités de surveillance de Zabbix, vous devez installer l'agent Zabbix sur les machines cibles que vous souhaitez surveiller.

Conditions préalables,

Avant de procéder à l'installation, assurez-vous d'avoir les prérequis suivants :

Connectivité réseau au serveur Zabbix.

Toutes les actions sont éffectées depuis la session root :

- Étape 1 : Mettre à jour les packages système.

Tout d'abord, nous devons nous assurer que notre système est à jour. 

Ouvrez un terminal ou SSH dans votre système Debian et exécutez la commande suivante :
```
apt update && apt upgrade -y 
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

Maintenant, que les dépôts ont été configurés, nous pouvons installer l'agent Zabbix en exécutant la commande suivante.

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

Si vous avez un pare-feu UFW en cours d’exécution sur la machine à surveiller, autorisez les ports 10050 & 10051.
