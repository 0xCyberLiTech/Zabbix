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

<p align="center">
  🔒 CyberSécurité par <a href="https://github.com/0xCyberLiTech">0xCyberLiTech</a> • Made with 💻
</p>

