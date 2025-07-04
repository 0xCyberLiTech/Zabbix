![zabbix-logo](./images/zabbix-logo.png)

## ZABBIX - Mise à jour de Zabbix 7.2 vers 7.4 sur Debian (MariaDB + Apache2).

## Sommaire :

🧱 Prérequis

- Zabbix 7.2 déjà installé
- Apache2 comme serveur web
- MariaDB (ex. mariadb-server)
- Accès root ou sudo

1. 🔐 Sauvegardes indispensables
--------------------------------
🔸 a. Sauvegarde de la base Zabbix

```bash
mysqldump -u root -p zabbix > /root/zabbix_backup_7.2.sql
```

🔸 b. Sauvegarde des fichiers de configuration

```bash
cp /etc/zabbix/zabbix_server.conf /etc/zabbix/zabbix_server.conf.bak
```

```bash
cp -r /etc/zabbix/web /etc/zabbix/web.bak
```

2. 📦 Mise à jour du dépôt Zabbix vers 7.4
-------------------------------------------
🔸 a. Supprimez l’ancien dépôt (facultatif mais conseillé)

```bash
sudo rm /etc/apt/sources.list.d/zabbix.list
```

🔸 b. Ajoutez le dépôt 7.4 correspondant à votre version Debian

Exemple pour Debian 12 (Bookworm) :

```bash
wget https://repo.zabbix.com/zabbix/7.4/debian/pool/main/z/zabbix-release/zabbix-release_7.4-1+debian12_all.deb
```

```bash
sudo dpkg -i zabbix-release_7.4-1+debian12_all.deb
```

```bash
sudo apt update
```

Si certains paquets sont marqués "retenus" (kept back), utilisez :

```bash
sudo apt full-upgrade
```

4. 🔄 Mise à jour de la base de données :
------------------------------------------
Zabbix mettra automatiquement à jour le schéma à la première exécution.

```bash
sudo systemctl restart zabbix-server
```

Vérifiez les logs :

```bash
tail -f /var/log/zabbix/zabbix_server.log
```

Vous devez voir :

```
database upgrade in progress
database upgrade completed
```

5. 🌐 Redémarrer les services Web et Agent
-------------------------------------------

```bash
sudo systemctl restart apache2
```

```bash
sudo systemctl restart zabbix-agent
```

6. ✅ Vérifier la version finale
---------------------------------

Dans l'interface web : Administration > About

Ou via terminal :

```bash
zabbix_server -V | head -n 1
```

7. ✅ Script automatisé pour la migration de zabbix 7.x vers 7.y
-----------------------------------------------------------------

Créé un fichier vide nommé upgrade_zabbix_7.2_to_7.4.sh

```bash
touch upgrade_zabbix_7.2_to_7.4.sh
```

Injecter le code ci-dessous dans ce fichier upgrade_zabbix_7.2_to_7.4.sh

```
#!/bin/bash

set -e

# Variables
ZABBIX_DB_NAME="zabbix"
ZABBIX_DB_BACKUP="/root/zabbix_backup_$(date +%F_%H-%M).sql"
DEBIAN_VERSION=$(lsb_release -cs)
ZABBIX_RELEASE_PKG="zabbix-release_7.4-1+debian12_all.deb"
ZABBIX_REPO_URL="https://repo.zabbix.com/zabbix/7.4/debian/pool/main/z/zabbix-release/${ZABBIX_RELEASE_PKG}"

echo "=== 📦 Mise à jour de Zabbix 7.2 vers 7.4 ==="

# 1. Sauvegarde de la base MariaDB
echo "🔐 Sauvegarde de la base de données MariaDB..."
mysqldump -u root -p ${ZABBIX_DB_NAME} > ${ZABBIX_DB_BACKUP}
echo "✅ Sauvegarde SQL dans : ${ZABBIX_DB_BACKUP}"

# 2. Sauvegarde des fichiers de configuration
echo "🗃 Sauvegarde des fichiers de configuration..."
cp /etc/zabbix/zabbix_server.conf /etc/zabbix/zabbix_server.conf.bak
cp -r /etc/zabbix/web /etc/zabbix/web.bak
echo "✅ Fichiers de configuration sauvegardés"

# 3. Mise à jour du dépôt
echo "🌐 Mise à jour du dépôt Zabbix 7.4..."
rm -f /etc/apt/sources.list.d/zabbix.list
wget ${ZABBIX_REPO_URL}
dpkg -i ${ZABBIX_RELEASE_PKG}
apt update

# 4. Mise à jour des paquets
echo "📥 Mise à jour des paquets Zabbix..."
apt -y upgrade zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent

# 5. Redémarrage des services
echo "🔄 Redémarrage de zabbix-server pour mise à jour de la base..."
systemctl restart zabbix-server

echo "⏳ Attente 10s pour la mise à jour de la base..."
sleep 10
tail -n 20 /var/log/zabbix/zabbix_server.log

# 6. Redémarrage Apache et Agent
echo "🚀 Redémarrage d'Apache et de l'agent Zabbix..."
systemctl restart apache2
systemctl restart zabbix-agent

# 7. Affichage de la version finale
echo "✅ Version actuelle de Zabbix :"
zabbix_server -V | head -n 1

echo "🎉 Mise à jour de Zabbix 7.2 → 7.4 terminée avec succès !"

```
```bash
chmod +x upgrade_zabbix_7.2_to_7.4.sh
```

<p align="center">
  🔒 CyberSécurité par <a href="https://github.com/0xCyberLiTech">0xCyberLiTech</a> • Made with 💻
</p>

