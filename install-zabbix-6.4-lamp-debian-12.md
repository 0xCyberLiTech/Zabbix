
![zabbix-logo](./images/zabbix-logo.png)
# Installation ZABBIX 6.4.4 + LAMP + DEBIAN 12 + zabbix-agent2.

## Sommaire :

Commençons par installer notre serveur LAMP.

| Cat | Etapes |
|------|------| 
| - A. | [Installer et configurer apache2.](#balise_01) |
| - a1. | [Exemple pour la création de deux virtualhosts HTTP & HTTPS apache2.](https://github.com/0xCyberLiTech/Apache2/blob/main/Exemple_create_VirtualHost.md) |
| - B. | [Installer et configurer PHP](#balise_02) |
| - C. | [Installer et configurer MySQL (MariaDB)](#balise_03) |

```
apt -y install apache2
```
```
systemctl start apache2.service
```
```
systemctl enable apache2.service
```
```
systemctl status apache2.service
```
Install PHP

La version par défaut de PHP à installer sur Debian ne serait pas la plus récente.
Par exemple, en faisant cet article, pour la version 12 de Bookworm, le PHP était 8.2, et pour 11 Bullseye- PHP7.4.

apt install php

