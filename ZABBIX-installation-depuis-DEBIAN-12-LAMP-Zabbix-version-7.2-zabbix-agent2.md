
<p align="center">
  <img src="./images/zabbix-logo.png" alt="zabbix-logo" width="200"/>
</p>

# 🚀 Installation de ZABBIX 7.2 sur DEBIAN 12 (LAMP + zabbix-agent2)

---

## 📑 Sommaire

<details>
<summary><strong>Afficher / Masquer le sommaire</strong></summary>

- [Prérequis](#prérequis)
- [Mise à jour du système](#mise-à-jour-du-système)
- [Installer le serveur Apache2](#balise_01)
- [Créer deux VirtualHosts HTTP & HTTPS](https://github.com/0xCyberLiTech/Apache2/blob/main/Cr%C3%A9%C3%A9-deux-VirtualHosts-HTTP-HTTPS.md)
- [Installer PHP](#balise_02)
- [Installer MySQL (MariaDB)](#balise_03)
- [Installer ZABBIX 7.2](#balise_04)
- [Configurer et démarrer l'agent ZABBIX 2](#balise_05)
- [Configuration du firewall (UFW)](#concernant-lutilisation-dun-firewall-ufw-sur-votre-serveur-zabbix)

</details>

---

## 🛠️ Prérequis

---

## 🔄 Mise à jour du système

```bash
apt update && apt upgrade -y
```

---

## 💡 Commençons par installer notre serveur LAMP

1. [Installer le serveur Apache2.](#balise_01)
2. [Créer deux VirtualHosts HTTP & HTTPS.](https://github.com/0xCyberLiTech/Apache2/blob/main/Cr%C3%A9%C3%A9-deux-VirtualHosts-HTTP-HTTPS.md)
3. [Installer PHP.](#balise_02)
4. [Installer MySQL (MariaDB)](#balise_03)
5. [Installer ZABBIX 7.2](#balise_04)
6. [Configurer et démarrer l'agent ZABBIX 2](#balise_05)

---

<a name="balise_01"></a>

## 🖥️ Installer le serveur Apache2

```bash
apt -y install apache2
```
```bash
systemctl start apache2.service
systemctl enable apache2.service
systemctl status apache2.service
```

---

<a name="balise_02"></a>
## 🐘 Installer PHP

Pour DEBIAN 12 (Bookworm), la version de PHP est 8.2.
Pour DEBIAN 11 (Bullseye), la version de PHP est 7.4.
```
apt install php
```
## Installation de PHP-FPM
````
apt -y install php-fpm
````
Ajoutez les paramètres dans le ou les Virtualhosts que vous souhaitez associer à PHP-FPM.
```
nano /etc/apache2/sites-enabled/000-default.conf
```
```
<VirtualHost *:80>
        <FilesMatch \.php$>
                SetHandler "proxy:unix:/var/run/php/php8.2-fpm.sock|fcgi://localhost/"
        </FilesMatch>
```
```
a2enmod proxy_fcgi setenvif
```
```
Considering dependency proxy for proxy_fcgi:
Enabling module proxy.
Enabling module proxy_fcgi.
Module setenvif already enabled
To activate the new configuration, you need to run:
  systemctl restart apache2
```
```
systemctl restart apache2.service
```
```
a2enconf php8.2-fpm
```
```
Enabling conf php8.2-fpm.
To activate the new configuration, you need to run:
  systemctl reload apache2
```
```
systemctl restart php8.2-fpm apache2
```
Créez le fichier [info.php] dans la racine du dossier Web, ( /va/www/html/ ).
```
echo '<?php phpinfo(); ?>' > /var/www/html/info.php
```
Accéder à l'Url http://mon-ip-local/info.php afin de tester.

On peut constater que le module FPM esy pris en charge.

Serveur API <--> FPM/FastCGI

![zabbix-29.png](./images/php.png)

C'est Ok pour la prise en charge de FPM, passons à la suite.

<a name="balise_03"></a>
## Installation du serveur MariaDB (MySQL)

Nous devons exécuter la commande comme mentionné ci-dessous :
```
apt -y install mariadb-server
```
```
nano /etc/mysql/mariadb.conf.d/50-server.cnf
```
```
character-set-server  = utf8mb4
collation-server      = utf8mb4_general_ci
```
```
systemctl restart mariadb.service
```
Sécuriser le serveur MariaDB :
```
mysql_secure_installation
```
```
NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user. If you've just installed MariaDB, and
haven't set the root password yet, you should just press enter here.

Enter current password for root (enter for none):
OK, successfully used password, moving on...

Setting the root password or using the unix_socket ensures that nobody
can log into the MariaDB root user without the proper authorisation.

You already have your root account protected, so you can safely answer 'n'.

Switch to unix_socket authentication [Y/n] Y
Enabled successfully!
Reloading privilege tables..
 ... Success!


You already have your root account protected, so you can safely answer 'n'.

Change the root password? [Y/n] Y
New password:
Re-enter new password:
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] Y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] Y
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] Y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] Y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```
```
systemctl restart mariadb.service
```
Se connecter à MySQL :
```
mysql
```
- [Unix_Socket] authentication is enabled by default :
```
MariaDB [(none)]> show grants for root@localhost;
```
```
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Grants for root@localhost                                                                                                                                                 |
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| GRANT ALL PRIVILEGES ON *.* TO `root`@`localhost` IDENTIFIED VIA mysql_native_password USING '*2538008A2BB77A6ACB52279941975854BA874015' OR unix_socket WITH GRANT OPTION |
| GRANT PROXY ON ''@'%' TO 'root'@'localhost' WITH GRANT OPTION                                                                                                             |
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
2 rows in set (0,000 sec)
```
- Show user list :
```
MariaDB [(none)]> select user,host,password from mysql.user; 
```
```
+-------------+-----------+-------------------------------------------+
| User        | Host      | Password                                  |
+-------------+-----------+-------------------------------------------+
| mariadb.sys | localhost |                                           |
| root        | localhost | *2538008A2BB77A6ACB52279941975854BA874015 |
| mysql       | localhost | invalid                                   |
+-------------+-----------+-------------------------------------------+
3 rows in set (0,001 sec)
```
- Show database list :
```
MariaDB [(none)]> show databases;
```
```
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0,000 sec)
```
## Create test database :
```
MariaDB [(none)]> create database test_database; 
Query OK, 1 row affected (0.000 sec)
```
- Create test table on test database :
```
MariaDB [(none)]> create table test_database.test_table (id int, name varchar(50), address varchar(50), primary key (id)); 
Query OK, 0 rows affected (0.108 sec)
```
- Insert data to test table :
```
MariaDB [(none)]> insert into test_database.test_table(id, name, address) values("001", "Debian", "Hiroshima"); 
Query OK, 1 row affected (0.036 sec)
```
- Show test table :
```
MariaDB [(none)]> select * from test_database.test_table;
```
```
+----+--------+-----------+
| id | name   | address   |
+----+--------+-----------+
|  1 | Debian | Hiroshima |
+----+--------+-----------+
1 row in set (0.000 sec)
```
- Delete test database :
```
MariaDB [(none)]> drop database test_database; 
Query OK, 1 row affected (0.111 sec)
```
```
MariaDB [(none)]> exit;
```
```
Bye
```
Si vous souhaitez supprimer toutes les données de MariaDB et l'initialiser, exécutez comme suit.
```
systemctl stop mariadb
```
```
rm -rf /var/lib/mysql/*
```
```
mysql_install_db --datadir=/var/lib/mysql --user=mysql
```
```
systemctl start mariadb
```
<a name="balise_04"></a>
## Installer Zabbix dans ça dernière version stable 7.2 à ce jour (28-06-2025).

- Avant de commencer il faut installer et configurer (NTPsec).

[Installer et configurer (NTPsec)](https://github.com/0xCyberLiTech/NTPsec/blob/main/Installer-et-configurer-NTPsec.md)

- Ajoutez les derniers dépôts stable pour Zabbix 7.2.

Pour surveiller Zabbix lui-même, il faudra également installer l'agent Zabbix 2 sur ce serveur et configurer celui-ci.

Suivre la version des derniers dépôts (en prod 7.2) : https://repo.zabbix.com/zabbix/7.2/ à ce jou 28-06-2025.

Zabbix Official Repository
```
wget https://repo.zabbix.com/zabbix/7.2/release/debian/pool/main/z/zabbix-release/zabbix-release_latest_7.2+debian12_all.deb
```
```
dpkg -i zabbix-release_latest_7.2+debian12_all.deb
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
deb https://repo.zabbix.com/zabbix/7.2/stable/debian bookworm main
deb-src https://repo.zabbix.com/zabbix/7.2/stable/debian bookworm main
```
```
apt update
```
```
apt -y install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent2 php-mysql php-gd php-bcmath php-net-socket
```
Créez une base de données nommée zabbix.
```
mysql
```
```
create database zabbix character set utf8mb4 collate utf8mb4_bin;
```
Remplacez-le [mot de passe] par le mot de passe de votre choix :
```
grant all privileges on zabbix.* to zabbix@'localhost' identified by 'zabbix';
```
```
set global log_bin_trust_function_creators = 1;
```
exit;
```

```bash
# Importer le schéma de la base de données Zabbix (saisir le mot de passe défini précédemment)
zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql -uzabbix -p zabbix
```

> 💡 **Astuce :** Utilisez la commande ci-dessus pour initialiser la base de données Zabbix. Si vous voyez des erreurs, vérifiez les droits de l'utilisateur et le mot de passe.

---

### ⚙️ Configuration du serveur Zabbix

Éditez le fichier de configuration principal :
```bash
nano /etc/zabbix/zabbix_server.conf
```
Modifiez les paramètres suivants :
```ini
DBName=zabbix
DBUser=zabbix
DBPassword=zabbix
```
Enregistrez et quittez nano (`Ctrl+O` puis `Ctrl+X`).

---

Redémarrez et activez le service Zabbix Server :
```bash
systemctl restart zabbix-server.service
systemctl enable zabbix-server.service
```

---

<a name="balise_05"></a>
## 🕵️‍♂️ Configurez et démarrez l'agent Zabbix pour surveiller le serveur Zabbix lui-même

1. **Éditez la configuration de l'agent :**
   ```bash
   nano /etc/zabbix/zabbix_agent2.conf
   ```
   Modifiez/ajoutez :
   ```ini
   Server=127.0.0.1
   ServerActive=127.0.0.1
   Hostname=Zabbix server
   AllowKey=system.run[*]
   #Plugins.SystemRun.LogRemoteCommands=1
   ```
   > 💡 **Astuce :** `AllowKey` permet d'autoriser l'exécution de commandes à distance depuis l'interface Zabbix.

2. **Redémarrez l'agent :**
   ```bash
   systemctl restart zabbix-agent2.service
   ```

---

### 🛠️ Ajustez la configuration PHP pour Zabbix

1. **Éditez le pool PHP-FPM**
   ```bash
   nano /etc/php/8.2/fpm/pool.d/www.conf
   ```
   Ajoutez à la fin :
   ```ini
   php_value[max_execution_time] = 300
   php_value[memory_limit] = 128M
   php_value[post_max_size] = 16M
   php_value[upload_max_filesize] = 2M
   php_value[max_input_time] = 300
   php_value[max_input_vars] = 10000
   php_value[always_populate_raw_post_data] = -1
   php_value[date.timezone] = Europe/Paris
   ```
   Enregistrez et quittez (`Ctrl+O` puis `Ctrl+X`).

2. **Redémarrez PHP-FPM et Apache2 :**
   ```bash
   systemctl restart php8.2-fpm apache2
   ```

---

### 🔍 Vérifiez la configuration Apache pour Zabbix

1. **Éditez le fichier de conf :**
   ```bash
   nano /etc/apache2/conf-enabled/zabbix.conf
   ```
   Vérifiez que les paramètres PHP sont bien présents dans les blocs `<IfModule mod_php.c>` et `<IfModule mod_php7.c>`.

2. **Redémarrez tous les services nécessaires :**
   ```bash
   systemctl restart zabbix-server zabbix-agent2 apache2
   systemctl enable zabbix-server zabbix-agent2 apache2
   ```

3. **Consultez les logs de l'agent Zabbix (optionnel) :**
   ```bash
   tail -100f /var/log/zabbix/zabbix_agent2.log
   ```

---

## 🌐 Finalisation de l'installation via l'interface web

Rendez-vous sur : [http://mon-ip-local/zabbix](http://mon-ip-local/zabbix)

Suivez les étapes graphiques d'installation :

| Étape | Capture d'écran |
|-------|----------------|
| 1     | ![Zabbix-7-001.png](./images/Zabbix-7-001.png) |
| 2     | ![Zabbix-7-002.png](./images/Zabbix-7-002.png) |
| 3     | ![Zabbix-7-003.png](./images/Zabbix-7-003.png) |
| 4     | ![Zabbix-7-004.png](./images/Zabbix-7-004.png) |
| 5     | ![Zabbix-7-005.png](./images/Zabbix-7-005.png) |
| 6     | ![Zabbix-7-006.png](./images/Zabbix-7-006.png) |
| 7     | ![Zabbix-7-007.png](./images/Zabbix-7-007.png) |
| 8     | ![Zabbix-7-008.png](./images/Zabbix-7-008.png) |

---

## ⚠️ Important : Activation des scripts globaux Zabbix

Par défaut, la variable `EnableGlobalScripts` est désactivée dans `/etc/zabbix/zabbix_server.conf`.

![script.png](./images/script.png)

1. **Éditez le fichier**
   ```bash
   nano /etc/zabbix/zabbix_server.conf
   ```
2. **Modifiez la section suivante**
   ```ini
   ### Option: EnableGlobalScripts
   #    Enable global scripts on Zabbix server.
   #       0 - disable
   #       1 - enable
   #
   # Mandatory: no
   # Default:
   # EnableGlobalScripts=1
   EnableGlobalScripts=1
   ```
   Enregistrez et quittez (`Ctrl+O` puis `Ctrl+X`).

3. **Redémarrez les services**
   ```bash
   systemctl restart zabbix-server zabbix-agent2 apache2
   ```

---

## 🔒 Sécurisation et firewall (UFW)

Pour plus de détails sur UFW, consultez [ce guide](https://github.com/0xCyberLiTech/Cybersecurite/blob/main/UFW-installation-et-configuration.md).

### Exemples de règles UFW pour Zabbix

```bash
# Autoriser SSH uniquement depuis une IP précise
ufw limit in on enp86s0 from 192.168.50.118 to 192.168.50.250 port 2277 proto tcp comment '2277 SSH'

# Autoriser HTTP/HTTPS
ufw allow in on enp86s0 from 192.168.50.118 to 192.168.50.250 port 80 proto tcp comment '80 Apache2'
ufw allow in on enp86s0 from 192.168.50.118 to 192.168.50.250 port 443 proto tcp comment '443 Apache2'

# Autoriser les ports Zabbix (passif/actif)
ufw allow in on enp86s0 from 192.168.0.0/16 to 192.168.50.250 port 10050 proto tcp comment '1050 agent Zabbix - For Passive checks'
ufw allow in on enp86s0 from 192.168.0.0/16 to 192.168.50.250 port 10051 proto tcp comment '1051 agent Zabbix - For Active checks'

# Lister les règles actives
ufw status numbered
```

> 💡 **Astuce :** La variable `limit` permet de limiter le nombre de tentatives de connexion SSH pour renforcer la sécurité.

---

## Concernant l'utilisation d'un firewall (UFW) sur votre serveur Zabbix :

[Vous pouvez obtenir plus de détail sur UFW ici.](https://github.com/0xCyberLiTech/Cybersecurite/blob/main/UFW-installation-et-configuration.md)

Ouvrir le port SSH approprié en entrée, afin d'avoir la main sur votre serveur Zabbix à distance.

Dans cet exemple, je n'autorise que la machine distante 192.168.50.118 à pouvoir accéder en SSH sur le serveur Zabbix au travers du port 2277 en TCP en entrée.
```
ufw limit in on enp86s0 from 192.168.50.118 to 192.168.50.250 port 2277 proto tcp comment '2277 SSH'
```
La variable 'limit' correspond à n'autoriser que 6 tentatives de connexion en 30 secondes sur notre règle. 

Cela permet de renfocer un peu plus la sécurité.

Ouvrir le port 80 sur le serveur Zabbix ainsi que le port 443 en entrée.
```
ufw allow in on enp86s0 from 192.168.50.118 to 192.168.50.250 port 80 proto tcp comment '80 Apache2'
```
```
ufw allow in on enp86s0 from 192.168.50.118 to 192.168.50.250 port 443 proto tcp comment '443 Apache2'
```
- Il faut autoriser le LANSUBNET 192.168.0.0/16 à communiquer vers le serveur Zabbix (192.168.50.250) à travers le port 10050 en TCP, pour le mode passif.
- Il faut autoriser le LANSUBNET 192.168.0.0/16 à communiquer vers le serveur Zabbix (192.168.50.250) à travers le port 10051 en TCP, pour le mode actif.

Ces ports doivent être ouverts en entrée sur le serveur Zabbix, afin de recueillir les communications en provenance des agent Zabbix des hôtes distants, que ce soit en mode passif ou en mode actif.
```
ufw allow in on enp86s0 from 192.168.0.0/16 to 192.168.50.250 port 10050 proto tcp comment '1050 agent Zabbix - For Passive checks'
```
```
ufw allow in on enp86s0 from 192.168.0.0/16 to 192.168.50.250 port 10051 proto tcp comment '1051 agent Zabbix - For Active checks'
```
Lister les règles en service :
```
ufw status numbered
```
```
     To                         Action      From
     --                         ------      ----
[ 1] 192.168.50.250 2277/tcp on enp86s0 ALLOW IN    192.168.50.118             # 2277 SSH
[ 2] 192.168.50.250 80/tcp on enp86s0 ALLOW IN    192.168.50.118             # 80 Apache2
[ 3] 192.168.50.250 443/tcp on enp86s0 ALLOW IN    192.168.50.118             # 443 Apache2
[ 4] 192.168.50.250 10050/tcp on enp86s0 ALLOW IN    192.168.0.0/16             # 1050 agent Zabbix - For Passive checks
[ 5] 192.168.50.250 10051/tcp on enp86s0 ALLOW IN    192.168.0.0/16             # 1051 agent Zabbix - For Active checks
```
