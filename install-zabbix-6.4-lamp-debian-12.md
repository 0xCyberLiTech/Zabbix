
![zabbix-logo](./images/zabbix-logo.png)
# Installation ZABBIX 6.4.4 + LAMP + DEBIAN 12 + zabbix-agent2.

## Sommaire :

Prérequis :

Mise à jour du système :
```
apt update && apt upgrade -y
```
Commençons par installer notre serveur LAMP.

| Cat | Etapes |
|------|------| 
| - A. | [Installer apache2.](#balise_01) |
| - a1. | [Exemple pour la création de deux virtualhosts HTTP & HTTPS apache2.](https://github.com/0xCyberLiTech/Apache2/blob/main/Exemple_create_VirtualHost.md) |
| - B. | [Installer PHP](#balise_02) |
| - C. | [Installer MySQL (MariaDB)](#balise_03) |

<a name="balise-01"></a>

# Installation du serveur Apache2 :

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
# Installation de PHP :

Pour DEBIAN 12 (Bookworm), la version de PHP est 8.2.
Pour DEBIAN 11 (Bullseye), la version de PHP est 7.4.
```
apt install php
```

# Installation de PHP-FPM
````
apt -y install php-fpm
````
Ajoutez les paramètres dans Virtualhost que vous souhaitez définir PHP-FPM.
```
# add into <VirtualHost> - </VirtualHost>
    <FilesMatch \.php$>
        SetHandler "proxy:unix:/var/run/php/php8.2-fpm.sock|fcgi://localhost/"
    </FilesMatch>
</VirtualHost>
```
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
Créez [phpinfo] dans la racine Web de Virtualhost, vous définissez PHP-FPM et y accédez, alors c'est OK si [FPM/FastCGI] est affiché.
```
echo '<?php phpinfo(); ?>' > /var/www/html/info.php
```
Accéder à l'Url http://mon-ip-local/info.php afin de tester.

On peut constater que le module FPM esy pris en charge.

Server API <--> FPM/FastCGI

![zabbix-29.png](./images/zabbix-29.png)

C'est OK il si trouve.

