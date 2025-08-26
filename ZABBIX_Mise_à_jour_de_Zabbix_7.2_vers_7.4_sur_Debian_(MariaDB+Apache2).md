<div align="center">

  <br></br>
  
  <a href="https://github.com/0xCyberLiTech">
    <img src="https://readme-typing-svg.herokuapp.com?font=JetBrains+Mono&size=50&duration=6000&pause=1000000000&color=FF0048&center=true&vCenter=true&width=1100&lines=%3ESUPERVISION+ZABBIX_" alt="Titre dynamique SUPERVISION ZABBIX" />
  </a>
  
  <br></br>

  <h2>Laboratoire numérique pour la cybersécurité, Linux & IT</h2>

  <p align="center">
    <a href="https://0xcyberlitech.github.io/">
      <img src="https://img.shields.io/badge/Portfolio-0xCyberLiTech-181717?logo=github&style=flat-square" alt="🌐 Portfolio" />
    </a>
    <a href="https://github.com/0xCyberLiTech">
      <img src="https://img.shields.io/badge/Profil-GitHub-181717?logo=github&style=flat-square" alt="🔗 Profil GitHub" />
    </a>
    <a href="https://github.com/0xCyberLiTech/Zabbix/releases/latest">
      <img src="https://img.shields.io/github/v/release/0xCyberLiTech/Zabbix?label=version&style=flat-square&color=blue" alt="📦 Dernière version" />
    </a>
    <a href="https://github.com/0xCyberLiTech/Zabbix/blob/main/CHANGELOG.md">
      <img src="https://img.shields.io/badge/📄%20Changelog-Zabbix-blue?style=flat-square" alt="📄 CHANGELOG Zabbix" />
    </a>
    <a href="https://github.com/0xCyberLiTech?tab=repositories">
      <img src="https://img.shields.io/badge/Dépôts-publics-blue?style=flat-square" alt="📂 Dépôts publics" />
    </a>
    <a href="https://github.com/0xCyberLiTech/Zabbix/graphs/contributors">
      <img src="https://img.shields.io/badge/👥%20Contributeurs-cliquez%20ici-007ec6?style=flat-square" alt="👥 Contributeurs Zabbix" />
    </a>
  </p>

</div>

<!--
Optimisation SEO : mots-clés cybersécurité, Linux, administration système, sécurité informatique, tutoriels, guides, expertise, formation, supervision, Docker, OpenVAS, firewall, proxy, DNS, SSH, Debian, IT, réseau, cryptographie, open source, ressources techniques, étudiants, professionnels, passionnés.
-->

<div align="center">
  <img src="https://img.icons8.com/fluency/96/000000/cyber-security.png" alt="CyberSec" width="80"/>
</div>

<div align="center">
  <p>
    <strong>Cybersécurité</strong> <img src="https://img.icons8.com/color/24/000000/lock--v1.png"/> • <strong>Linux Debian</strong> <img src="https://img.icons8.com/color/24/000000/linux.png"/> • <strong>Sécurité informatique</strong> <img src="https://img.icons8.com/color/24/000000/shield-security.png"/>
  </p>
</div>

---

## 🚀 À propos & Objectifs

Ce projet propose des solutions innovantes et accessibles en cybersécurité, avec une approche centrée sur la simplicité d’utilisation et l’efficacité. Il vise à accompagner les utilisateurs dans la protection de leurs données et systèmes, tout en favorisant l’apprentissage et le partage des connaissances.

Le contenu est structuré, accessible et optimisé SEO pour répondre aux besoins de :
- 🎓 Étudiants : approfondir les connaissances
- 👨‍💻 Professionnels IT : outils et pratiques
- 🖥️ Administrateurs système : sécuriser l’infrastructure
- 🛡️ Experts cybersécurité : ressources techniques
- 🚀 Passionnés du numérique : explorer les bonnes pratiques

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

<div align="center">
  <a href="https://github.com/0xCyberLiTech" target="_blank" rel="noopener">
    <img src="https://skillicons.dev/icons?i=linux,debian,bash,docker,nginx,git,vim,python,markdown" alt="Skills" width="440">
  </a>
</div>

<p align="center">
  <b>🔒 Un guide proposé par <a href="https://github.com/0xCyberLiTech">0xCyberLiTech</a> • Pour une supervision accessible et efficace 🔒</b>
</p>
