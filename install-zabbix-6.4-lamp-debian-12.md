
![zabbix-logo](./images/zabbix-logo.png)
# Installation ZABBIX 6.4.4 + LAMP + DEBIAN 12 + zabbix-agent2.

## Sommaire :

Commençons par installer notre serveur LAMP.

| Cat | Etapes |
|------|------| 
| - A. | [Installer et configurer apache2.](#balise_01) |
| - a1. | [Installer et configurer apache2.](#balise_01) |

| - B. | [Installer et configurer PHP](#balise_02) |
| - C. | [Installer et configurer MySQL (MariaDB)](#balise_03) |


apt -y install apache2
systemctl start apache2.service
systemctl enable apache2.service
systemctl status apache2.service

Configure Apache2.

nano /etc/apache2/conf-enabled/security.conf

#ServerTokens OS
ServerTokens Prod
