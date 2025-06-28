![zabbix-logo](./images/zabbix-logo.png)

# 🚀 Installation de ZABBIX 7.2 sur Debian 12 (LAMP + zabbix-agent2)

---

> **Dernière mise à jour :** 28/06/2025  
> **Distribution cible :** Debian 12 "Bookworm"  
> **Composants :** Apache2, PHP 8.2, MariaDB, Zabbix 7.2, zabbix-agent2

---

## 📚 Sommaire

- [Prérequis](#prérequis)
- [Mise à jour du système](#mise-à-jour-du-système)
- [Installation du serveur LAMP](#installation-du-serveur-lamp)
    - [1. Installer Apache2](#1-installer-apache2)
    - [2. Créer des VirtualHosts HTTP & HTTPS](#2-créer-des-virtualhosts-http--https)
    - [3. Installer PHP 8.2 et PHP-FPM](#3-installer-php-82-et-php-fpm)
    - [4. Installer MariaDB](#4-installer-mariadb)
- [Installation de Zabbix 7.2](#installation-de-zabbix-72)
- [Configuration de l’agent Zabbix 2](#configuration-de-lagent-zabbix-2)
- [Configuration de PHP et Apache pour Zabbix](#configuration-de-php-et-apache-pour-zabbix)
- [Finalisation de l’installation (Web UI)](#finalisation-de-linstallation-web-ui)
- [Configuration avancée Zabbix 7.2](#configuration-avancée-zabbix-72)
- [Firewall (UFW)](#firewall-ufw)
- [Sources utiles](#sources-utiles)

---

## Prérequis

- Serveur Debian 12 fraîchement installé
- Accès root ou sudo
- Connexion Internet

---

## Mise à jour du système

```bash
sudo apt update && sudo apt upgrade -y
```

---

## Installation du serveur LAMP

### 1. Installer Apache2 <a name="1-installer-apache2"></a>

```bash
sudo apt install -y apache2
sudo systemctl enable --now apache2
sudo systemctl status apache2
```

---

### 2. Créer des VirtualHosts HTTP & HTTPS <a name="2-créer-des-virtualhosts-http--https"></a>

👉 [Voir le tuto dédié VirtualHosts](https://github.com/0xCyberLiTech/Apache2/blob/main/Cr%C3%A9%C3%A9-deux-VirtualHosts-HTTP-HTTPS.md)

---

### 3. Installer PHP 8.2 et PHP-FPM <a name="3-installer-php-82-et-php-fpm"></a>

```bash
sudo apt install -y php php-fpm
```

**Configurer PHP-FPM avec Apache2 :**
- Ajouter dans votre VirtualHost :
    ```apache
    <FilesMatch \.php$>
        SetHandler "proxy:unix:/var/run/php/php8.2-fpm.sock|fcgi://localhost/"
    </FilesMatch>
    ```
- Activer les modules nécessaires :
    ```bash
    sudo a2enmod proxy_fcgi setenvif
    sudo systemctl restart apache2
    sudo a2enconf php8.2-fpm
    sudo systemctl restart php8.2-fpm apache2
    ```

**Test PHP :**
```bash
echo '<?php phpinfo(); ?>' | sudo tee /var/www/html/info.php
```
> Accédez à http://votre-ip/info.php pour vérifier la prise en charge de PHP-FPM

---

### 4. Installer MariaDB <a name="4-installer-mariadb"></a>

```bash
sudo apt install -y mariadb-server
sudo systemctl enable --now mariadb
sudo mysql_secure_installation
```
> Répondez aux questions pour sécuriser MariaDB (mot de passe root, suppression des utilisateurs anonymes, etc.)

**Création d’une base de test (optionnel) :**
```sql
sudo mysql
CREATE DATABASE test_database;
USE test_database;
CREATE TABLE test_table (id INT PRIMARY KEY, name VARCHAR(50), address VARCHAR(50));
INSERT INTO test_table VALUES (1, 'Debian', 'Hiroshima');
SELECT * FROM test_table;
DROP DATABASE test_database;
EXIT;
```

---

## Installation de Zabbix 7.2 <a name="installation-de-zabbix-72"></a>

**1. Installer NTPsec (recommandé) :**  
👉 [Tuto NTPsec](https://github.com/0xCyberLiTech/NTPsec/blob/main/Installer-et-configurer-NTPsec.md)

**2. Ajouter le dépôt officiel Zabbix :**
```bash
wget https://repo.zabbix.com/zabbix/7.2/release/debian/pool/main/z/zabbix-release/zabbix-release_latest_7.2+debian12_all.deb
sudo dpkg -i zabbix-release_latest_7.2+debian12_all.deb
sudo apt update
```

**3. Installer Zabbix & dépendances :**
```bash
sudo apt install -y zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent2 php-mysql php-gd php-bcmath php-net-socket
```

**4. Créer la base de données Zabbix :**
```sql
sudo mysql
CREATE DATABASE zabbix character set utf8mb4 collate utf8mb4_bin;
GRANT ALL PRIVILEGES ON zabbix.* TO zabbix@localhost IDENTIFIED BY 'zabbix';
SET GLOBAL log_bin_trust_function_creators = 1;
EXIT;
zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql -uzabbix -p zabbix
```

**5. Configurer Zabbix server :**
```bash
sudo nano /etc/zabbix/zabbix_server.conf
# Modifier :
DBName=zabbix
DBUser=zabbix
DBPassword=zabbix
```
```bash
sudo systemctl restart zabbix-server
sudo systemctl enable zabbix-server
```

---

## Configuration de l’agent Zabbix 2 <a name="configuration-de-lagent-zabbix-2"></a>

```bash
sudo nano /etc/zabbix/zabbix_agent2.conf
# Modifier :
Server=127.0.0.1
ServerActive=127.0.0.1
Hostname=Zabbix server
```
```bash
sudo systemctl restart zabbix-agent2
sudo systemctl enable zabbix-agent2
```

---

## Configuration de PHP et Apache pour Zabbix <a name="configuration-de-php-et-apache-pour-zabbix"></a>

**Adapter les valeurs PHP (exemple pour PHP-FPM):**
```bash
sudo nano /etc/php/8.2/fpm/pool.d/www.conf
# Ajouter à la fin :
php_value[max_execution_time] = 300
php_value[memory_limit] = 128M
php_value[post_max_size] = 16M
php_value[upload_max_filesize] = 2M
php_value[max_input_time] = 300
php_value[max_input_vars] = 10000
php_value[date.timezone] = Europe/Paris
```
```bash
sudo systemctl restart php8.2-fpm apache2
```

---

## Finalisation de l’installation (Web UI) <a name="finalisation-de-linstallation-web-ui"></a>

Accédez à [http://votre-ip/zabbix](http://votre-ip/zabbix) et suivez les étapes de l’assistant graphique :

**Phases d’installation :**
- ![Zabbix-7-001.png](./images/Zabbix-7-001.png)
- ![Zabbix-7-002.png](./images/Zabbix-7-002.png)
- ![Zabbix-7-003.png](./images/Zabbix-7-003.png)
- ... (ajoutez les autres captures d’écran)

---

## Configuration avancée Zabbix 7.2 <a name="configuration-avancée-zabbix-72"></a>

> ⚠️ **Important :**  
> Le paramètre `EnableGlobalScripts` est désactivé par défaut sur Zabbix 7.2.  
> Pensez à l’activer si vous souhaitez utiliser les scripts globaux.

```bash
sudo nano /etc/zabbix/zabbix_server.conf
# Avant :
EnableGlobalScripts=0
# Après :
EnableGlobalScripts=1
```

**Pour l’agent2 :**  
Ajoutez dans `/etc/zabbix/zabbix_agent2.conf` :
```
AllowKey=system.run[*]
#Plugins.SystemRun.LogRemoteCommands=1
```
Redémarrez les services :
```bash
sudo systemctl restart zabbix-server zabbix-agent2 apache2
```

---

## Firewall (UFW) <a name="firewall-ufw"></a>

👉 [Guide UFW complet](https://github.com/0xCyberLiTech/Cybersecurite/blob/main/UFW-installation-et-configuration.md)

**Exemple de règles :**
```bash
# Autoriser SSH depuis une IP précise
sudo ufw limit in on enp86s0 from 192.168.50.118 to 192.168.50.250 port 2277 proto tcp comment '2277 SSH'

# Autoriser HTTP/HTTPS
sudo ufw allow in on enp86s0 from 192.168.50.118 to 192.168.50.250 port 80 proto tcp comment '80 Apache2'
sudo ufw allow in on enp86s0 from 192.168.50.118 to 192.168.50.250 port 443 proto tcp comment '443 Apache2'

# Autoriser les ports Zabbix (passif/actif)
sudo ufw allow in on enp86s0 from 192.168.0.0/16 to 192.168.50.250 port 10050 proto tcp comment '1050 agent Zabbix - For Passive checks'
sudo ufw allow in on enp86s0 from 192.168.0.0/16 to 192.168.50.250 port 10051 proto tcp comment '1051 agent Zabbix - For Active checks'

# Vérifier les règles
sudo ufw status numbered
```

---

## Sources utiles <a name="sources-utiles"></a>

- [Documentation officielle Zabbix](https://www.zabbix.com/documentation/current/en/manual/installation/)
- [Docs Debian](https://wiki.debian.org/fr/)
- [Guide Apache2](https://github.com/0xCyberLiTech/Apache2)
- [Guide NTPsec](https://github.com/0xCyberLiTech/NTPsec)
- [Guide UFW Cybersecurité](https://github.com/0xCyberLiTech/Cybersecurite/blob/main/UFW-installation-et-configuration.md)

---

> _Tuto rédigé & maintenu par [0xCyberLiTech](https://github.com/0xCyberLiTech)_  
> _Retrouvez des tutos cybersécurité, Linux & DevOps sur mon GitHub !_
