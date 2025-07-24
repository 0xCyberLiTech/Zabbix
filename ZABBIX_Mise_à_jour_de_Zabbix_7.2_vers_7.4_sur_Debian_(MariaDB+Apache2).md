![zabbix-logo](./images/zabbix-logo.png)

<div align="center">

<a href="https://github.com/0xCyberLiTech">
  <img src="https://readme-typing-svg.herokuapp.com?font=Fira+Code&size=32&pause=1000&color=D14A4A&center=true&vCenter=true&width=650&lines=SUPERVISION+D'INFRASTRUCTURES;Monitorer+•+Analyser+•+Gérer;Zabbix+•+Nagios+•+Prometheus" alt="Typing SVG" />
</a>

<p align="center">
  <em>Un dépôt pédagogique sur la supervision des infrastructures numériques.</em><br>
  <strong>📊 Monitoring – 📈 Performance – ⚙️ Fiabilité</strong>
</p>

[![Dernière version](https://img.shields.io/github/v/release/0xCyberLiTech/Supervision?style=flat-square&color=blue)](https://github.com/0xCyberLiTech/Supervision/releases/latest)
[![Changelog](https://img.shields.io/badge/📄%20CHANGELOG-Supervision-blue)](https://github.com/0xCyberLiTech/Supervision/blob/main/CHANGELOG.md)
[![Langage](https://img.shields.io/badge/langage-Bash-blue)](https://bash.org/)
[![OS](https://img.shields.io/badge/système-Debian%2012-success)](https://www.debian.org/)
[![Licence](https://img.shields.io/github/license/0xCyberLiTech/Supervision)](LICENSE)
[![Statut](https://img.shields.io/badge/status-en%20développement-orange)]()

</div>

---

### 👨‍💻 **À propos de moi.**

> Bienvenue dans mon **laboratoire numérique personnel** dédié à l’apprentissage et à la vulgarisation de la cybersécurité.  
> Passionné par **Linux**, la **cryptographie** et les **systèmes sécurisés**, je partage ici mes notes, expérimentations et fiches pratiques.  
>  
> 🎯 **Objectif :** proposer un contenu clair, structuré et accessible pour étudiants, curieux et professionnels de l’IT.  
> 🔗 [Mon GitHub principal](https://github.com/0xCyberLiTech)

<p align="center">
  <a href="https://skillicons.dev">
    <img src="https://skillicons.dev/icons?i=linux,debian,bash,docker,nginx,git,vim" alt="Skills" />
  </a>
</p>

---

### 🎯 Objectif du dépôt

> Ce dépôt vise à centraliser les connaissances pratiques liées à la **supervision des systèmes d'information**. Il s’adresse à tous ceux souhaitant :
> 
> - Comprendre les enjeux du monitoring
> - Déployer des outils efficaces (Zabbix, Nagios, etc.)
> - Améliorer la **stabilité**, la **performance** et la **disponibilité** de leur infrastructure IT

---

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

---

**📅 Mise à jour :** Juillet 2025

---

<p align="center">
  <b>🔒 Un guide proposé par <a href="https://github.com/0xCyberLiTech">0xCyberLiTech</a> • Pour une supervision accessible et efficace 🔒</b>
</p>
