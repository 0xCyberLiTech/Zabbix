![zabbix-logo](./images/zabbix-logo.png)

images_build.yml
https://github.com/zabbix/zabbix-docker/actions/workflows/images_build.yml

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

Zabbix fournit des référentiels officiels contenant les packages nécessaires pour une installation et des mises à jour faciles. Exécutez les commandes suivantes pour ajouter le référentiel Zabbix et importer la clé GPG :

```
wget https://repo.zabbix.com/zabbix/6.4/debian/pool/main/z/zabbix-release/zabbix-release_6.4-1+debian12_all.deb
```
```
sudo dpkg -i zabbix-release_6.4-1+debian12_all.deb
```
Étape 3 : Installer l'agent Zabbix.

Maintenant que le référentiel est configuré, nous pouvons installer l'agent Zabbix en exécutant la commande suivante :

```
sudo apt update 
```
```
sudo apt install zabbix-agent
```
Lors de l'installation, vous pouvez être invité à configurer l'agent Zabbix. Si vous y êtes invité, sélectionnez « OK » ou appuyez sur Entrée pour continuer avec les valeurs par défaut.

- Étape 4 : Configurer l'agent Zabbix.

Une fois l'installation terminée, nous devons configurer l'agent Zabbix pour communiquer avec le serveur Zabbix.

Ouvrez le fichier de configuration de l'agent Zabbix à l'aide d'un éditeur de texte :

```
sudo nano /etc/zabbix/zabbix_agentd.conf
```
Dans le fichier de configuration, localisez les directives `Server=` et `ServerActive=` et définissez-les sur l'adresse IP ou le nom d'hôte de votre serveur Zabbix. Si vous n'êtes pas sûr, consultez votre administrateur de serveur Zabbix.

Enregistrez les modifications et quittez l'éditeur de texte.

- Étape 5 : Démarrer et activer l'agent Zabbix.

Pour démarrer l'agent Zabbix, exécutez la commande suivante :
```
sudo systemctl démarrer l'agent zabbix
```
Ensuite, activez l'agent Zabbix pour qu'il démarre automatiquement au démarrage du système :
```
sudo systemctl enable zabbix-agent
```
- Étape 6 : Vérifier l'état de l'agent Zabbix
Pour vous assurer que l'agent Zabbix est en cours d'exécution et communique avec le serveur Zabbix, utilisez la commande suivante :
```
sudo systemctl état zabbix-agent
```
Si l'agent s'exécute correctement, vous devriez voir un message d'état indiquant qu'il est actif et en cours d'exécution.
