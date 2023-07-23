# Sauvegarde de la base de donnée mysql zabbix :
```
mysqldump -u [username] -p [password] zabbix > zabbix_backup.sql
```
```
mysqldump -u root -p zabbix > zabbix_backup_23-07-2023_20-47.sql
```
```
mysqldump -u zabbix -p zabbix > zabbix_backup_23-07-2023_20-51.sql
```
# Restauration de la base de donnée mysql zabbix :
```
mysql -u [username] -p [password] zabbix < zabbix_backup.sql
```
```
mysql -u root -p zabbix < zabbix_backup_23-07-2023_20-47.sql
```
```
mysql -u zabbix -p zabbix < zabbix_backup_23-07-2023_20-51.sql
```
