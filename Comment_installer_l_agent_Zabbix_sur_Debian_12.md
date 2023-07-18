![zabbix-logo](./images/zabbix-logo.png)

# Comment installer l'agent Zabbix sur Debian 12

Zabbix est une solution de surveillance open source populaire utilisée par les administrateurs système pour surveiller et suivre les performances des serveurs, des réseaux et des applications. Pour utiliser efficacement les capacités de surveillance de Zabbix, vous devez installer l'agent Zabbix sur les machines cibles que vous souhaitez surveiller. Dans cet article, nous vous guiderons tout au long du processus d'installation de l'agent Zabbix sur un système basé sur Debian.

Conditions préalables,

Avant de procéder à l'installation, assurez-vous d'avoir les prérequis suivants :

Un système basé sur Debian (par exemple, Debian, Ubuntu ou leurs dérivés).

Connectivité réseau au serveur Zabbix.

- Étape 1 : Mettre à jour les packages système.

Tout d'abord, nous devons nous assurer que nos packages système sont à jour. 

Ouvrez un terminal ou SSH dans votre système Debian et exécutez la commande suivante :
```
sudo apt update && sudo apt upgrade -y 
```
Cette commande mettra à jour les listes de packages et mettra à niveau tous les packages obsolètes.

- Step 2 : Configurer Zabbix Repository

Zabbix fournit des dépôts officiels contenant les packages nécessaires pour une installation et des mises à jour faciles. 

Exécutez les commandes suivantes pour ajouter les dépôts Zabbix et importer la clé GPG :

Vérifier au préalable la dernière version de disponible depuis l'url suivante :

Valable pour DEBIAN 12 / 11 / 10 / 09

https://repo.zabbix.com/zabbix/6.4/debian/pool/main/z/zabbix-release/

Si la version de zabbix change (x.x) :

https://repo.zabbix.com/zabbix/x.x/debian/pool/main/z/zabbix-release/

Dernière version du paquet disponible au 13 juin 2023 pour DEBIAN 12 :

zabbix-release_6.4-1+debian12_all.deb

Pour DEBIAN 12
```
wget https://repo.zabbix.com/zabbix/6.4/debian/pool/main/z/zabbix-release/zabbix-release_6.4-1+debian12_all.deb
```
```
sudo dpkg -i zabbix-release_6.4-1+debian12_all.deb
```

Pour DEBIAN 11
```
wget https://repo.zabbix.com/zabbix/6.4/debian/pool/main/z/zabbix-release/zabbix-release_6.4-2+debian11_all.deb
```
```
sudo dpkg -i zabbix-release_6.4-2+debian11_all.deb
```
Les dépôts officiels suivants auront été ajoutés dans /etc/apt/sources.list.d/zabbix.list.

Je rappel que ce tuto est appliqué sur une DEBIAN 12 (bookworm).
```
cat /etc/apt/sources.list.d/zabbix.list

# Zabbix main repository
deb https://repo.zabbix.com/zabbix/6.4/debian bookworm main
deb-src https://repo.zabbix.com/zabbix/6.4/debian bookworm main

# Zabbix unstable repository
#deb https://repo.zabbix.com/zabbix/6.3/debian bookworm main
#deb-src https://repo.zabbix.com/zabbix/6.3/debian bookworm main
```
Étape 3 : Installer l'agent Zabbix.

Maintenant que les dépôts ont été configuré, nous pouvons installer l'agent Zabbix en exécutant la commande suivante :

```
sudo apt update 
```
```
sudo apt install zabbix-agent
```
Lors de l'installation, vous pouvez être invité à configurer l'agent Zabbix.

Si vous y êtes invité, sélectionnez « OK » ou appuyez sur Entrée pour continuer avec les valeurs par défaut.

- Étape 4 : Configurer l'agent Zabbix.

Une fois l'installation terminée, nous devons configurer l'agent Zabbix pour communiquer avec le serveur Zabbix.

Ouvrez le fichier de configuration de l'agent Zabbix à l'aide d'un éditeur de texte :

```
sudo nano /etc/zabbix/zabbix_agentd.conf
```
Dans le fichier de configuration, localisez les directives `Server=` et `ServerActive=` et définissez-les sur l'adresse IP ou le nom d'hôte de votre serveur Zabbix. Si vous n'êtes pas sûr, consultez votre administrateur de serveur Zabbix.

```
### Option: Server
#       List of comma delimited IP addresses, optionally in CIDR notation, or DNS names of Zabbix servers and Zabbix proxies.
#       Incoming connections will be accepted only from the hosts listed here.
#       If IPv6 support is enabled then '127.0.0.1', '::127.0.0.1', '::ffff:127.0.0.1' are treated equally
#       and '::/0' will allow any IPv4 or IPv6 address.
#       '0.0.0.0/0' can be used to allow any IPv4 address.
#       Example: Server=127.0.0.1,192.168.1.0/24,::1,2001:db8::/32,zabbix.example.com
#
# Mandatory: yes, if StartAgents is not explicitly set to 0
# Default:
# Server=

Server=192.168.50.250
```
```
### Option: ServerActive
#       Zabbix server/proxy address or cluster configuration to get active checks from.
#       Server/proxy address is IP address or DNS name and optional port separated by colon.
#       Cluster configuration is one or more server addresses separated by semicolon.
#       Multiple Zabbix servers/clusters and Zabbix proxies can be specified, separated by comma.
#       More than one Zabbix proxy should not be specified from each Zabbix server/cluster.
#       If Zabbix proxy is specified then Zabbix server/cluster for that proxy should not be specified.
#       Multiple comma-delimited addresses can be provided to use several independent Zabbix servers in parallel. Spaces are allowed.
#       If port is not specified, default port is used.
#       IPv6 addresses must be enclosed in square brackets if port for that host is specified.
#       If port is not specified, square brackets for IPv6 addresses are optional.
#       If this parameter is not specified, active checks are disabled.
#       Example for Zabbix proxy:
#               ServerActive=127.0.0.1:10051
#       Example for multiple servers:
#               ServerActive=127.0.0.1:20051,zabbix.domain,[::1]:30051,::1,[12fc::1]
#       Example for high availability:
#               ServerActive=zabbix.cluster.node1;zabbix.cluster.node2:20051;zabbix.cluster.node3
#       Example for high availability with two clusters and one server:
#               ServerActive=zabbix.cluster.node1;zabbix.cluster.node2:20051,zabbix.cluster2.node1;zabbix.cluster2.node2,zabbix.domain
#
# Mandatory: no
# Default:
# ServerActive=

ServerActive=192.168.50.250
```
Enregistrez les modifications et quittez l'éditeur de texte.

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
