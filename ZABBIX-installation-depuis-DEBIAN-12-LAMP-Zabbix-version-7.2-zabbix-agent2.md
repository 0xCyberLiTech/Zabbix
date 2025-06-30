
# 📡 Installation de Zabbix 7.2 sur Debian 12

> **Guide complet** pour déployer un serveur Zabbix 7.2 fonctionnel avec Apache2, PHP-FPM, MariaDB et Zabbix Agent 2 sur Debian 12.

![Zabbix Logo](./images/zabbix-logo.png)

---

## 📑 Sommaire

- [🔧 Prérequis](#-prérequis)
- [🌐 Apache2](#-1---installer-apache2)
- [🐘 PHP & PHP-FPM](#-2---installer-php--php-fpm)
- [🛢️ MariaDB (MySQL)](#-3---installer-mariadb-mysql)
- [📊 Zabbix Server + Frontend + Agent2](#-4---installer-zabbix-server--frontend--agent2)
- [👾 Agent Zabbix 2](#-5---installer-et-configurer-lagent-zabbix-2)
- [🧠 Infos Complémentaires](#-6---infos-complémentaires)

---

## 🔧 Prérequis

- ✅ Système Debian 12 à jour
- ✅ Apache2 opérationnel
- ✅ PHP et PHP-FPM installés
- ✅ Serveur MariaDB configuré
- ✅ NTP synchronisé

---

## 🌐 1 - Installer Apache2

```bash
apt -y install apache2
systemctl start apache2
systemctl enable apache2
```

---

## 🐘 2 - Installer PHP & PHP-FPM

```bash
apt install php php-fpm
```

Configurer le VirtualHost pour FPM :
```apacheconf
<VirtualHost *:80>
  <FilesMatch \.php$>
    SetHandler "proxy:unix:/var/run/php/php8.2-fpm.sock|fcgi://localhost/"
  </FilesMatch>
</VirtualHost>
```

Activer les modules et redémarrer :
```bash
a2enmod proxy_fcgi setenvif
a2enconf php8.2-fpm
systemctl restart php8.2-fpm apache2
```

---

## 🛢️ 3 - Installer MariaDB (MySQL)

```bash
apt -y install mariadb-server
mysql_secure_installation
```

Créer une base et utilisateur Zabbix :
```sql
CREATE DATABASE zabbix CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;
CREATE USER zabbix@localhost IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON zabbix.* TO zabbix@localhost;
```

---

## 📊 4 - Installer Zabbix Server + Frontend + Agent2

Ajouter les dépôts officiels :
```bash
wget https://repo.zabbix.com/zabbix/7.2/release/debian/pool/main/z/zabbix-release/zabbix-release_latest_7.2+debian12_all.deb
dpkg -i zabbix-release_latest_7.2+debian12_all.deb
apt update
```

Installer Zabbix :
```bash
apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent2
```

Configurer PHP et démarrer les services :
```bash
systemctl restart zabbix-server zabbix-agent2 apache2
systemctl enable zabbix-server zabbix-agent2 apache2
```

Interface Web : http://mon-ip-local/zabbix

---

## 👾 5 - Installer et configurer l'agent Zabbix 2

```bash
apt install zabbix-agent2 zabbix-agent2-plugin-mongodb zabbix-agent2-plugin-mssql zabbix-agent2-plugin-postgresql
systemctl restart zabbix-agent2
systemctl enable zabbix-agent2
```

---

## 🧠 6 - Infos Complémentaires

### 🔍 Problème `fping`

```bash
sudo ln -s /usr/bin/fping /usr/sbin/fping
sudo ln -s /usr/bin/fping6 /usr/sbin/fping6
```

### 📄 Logs de l’agent

```bash
tail -100f /var/log/zabbix/zabbix_agent2.log
```

### ⚙️ Exécution de scripts

```bash
nano /etc/zabbix/zabbix_server.conf
# EnableGlobalScripts=1

nano /etc/zabbix/zabbix_agent2.conf
# AllowKey=system.run[*]
```

### 🔐 Pare-feu UFW

```bash
ufw allow 10050/tcp comment 'Zabbix Passive'
ufw allow 10051/tcp comment 'Zabbix Active'
```

---

## 📂 Ressources GitHub associées

- 🔗 [Apache2 VirtualHosts](https://github.com/0xCyberLiTech/Apache2)
- 🔗 [Configuration NTPsec](https://github.com/0xCyberLiTech/NTPsec)
- 🔗 [Sécurisation avec UFW](https://github.com/0xCyberLiTech/Cybersecurite)

---

## 🧠 Auteur

Développé avec ❤️ par [0xCyberLiTech](https://github.com/0xCyberLiTech) · [Zabbix Guide 2025]
