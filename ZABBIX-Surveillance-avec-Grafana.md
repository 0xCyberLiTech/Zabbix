![zabbix-logo](./images/zabbix-logo.png)

# ZABBIX - Surveillance avec Grafana :

Pour tester si vous pouvez surveiller Zabbix avec Grafana, cliquez sur l’icône Explorer de Grafana comme indiqué dans la capture d’écran ci-dessous.

![grafana-01](./images/grafana-01.png)

Sélectionnez Zabbix dans le menu déroulant Explorer comme indiqué dans la capture d’écran ci-dessous.

![grafana-02](./images/grafana-02.png)

Maintenant, sélectionnez le type de données que vous souhaitez interroger à partir de Zabbix dans le menu déroulant Mode de requête, comme indiqué dans la capture d’écran ci-dessous.

Je vais sélectionner le type de métriques.

![grafana-03](./images/grafana-03.png)

Sélectionnez le groupe Zabbix souhaité dans la section Groupe. Je vais sélectionner le groupe de serveurs Zabbix par défaut.

![grafana-04](./images/grafana-04.png)

Sélectionnez votre hôte Zabbix dans la section Hôte. Je vais sélectionner l’hôte du serveur Zabbix.

![grafana-05](./images/grafana-05.png)

Sélectionnez une balise d’élément que vous souhaitez surveiller dans la section Balise d’élément.

Je vais sélectionner la balise item Application: Interface ens33 dans cet exemple.

Cette balise item vous permettra de surveiller l’interface réseau ens33.

![grafana-06](./images/grafana-06.png)

Maintenant, sélectionnez l’élément que vous souhaitez surveiller dans la section Élément.

Si vous avez sélectionné l’Application de balise Item : Interface ens33 comme je l’ai fait, vous pouvez sélectionner l’Interface Item ens33: Bit received pour surveiller la vitesse de téléchargement de l’interface réseau ens33.

![grafana-07](./images/grafana-07.png)

Vous devriez voir un graphique de la vitesse de téléchargement de l’interface réseau ens33.

![grafana-08](./images/grafana-08.png)

Concernat les règles de firewall (UFW) :

```
[ 1] 80/tcp                     ALLOW IN    192.168.50.0/24
[ 2] 443/tcp                    ALLOW IN    192.168.50.0/24
[ 3] 2234/tcp                   ALLOW IN    192.168.50.0/24
[ 4] 10050/tcp                  ALLOW IN    192.168.50.0/24
[ 5] 9443/tcp                   ALLOW IN    192.168.50.0/24
[ 6] 9392/tcp                   ALLOW IN    192.168.0.0/16
[ 7] 8181/tcp                   ALLOW IN    192.168.50.0/24
[ 8] 8585/tcp                   ALLOW IN    192.168.50.0/24
[ 9] 8086/tcp                   ALLOW IN    192.168.50.0/24
[10] 1883/tcp                   ALLOW IN    192.168.50.0/24
[11] 25/tcp                     ALLOW IN    192.168.50.0/24
[12] Anywhere                   ALLOW IN    172.21.0.0/16
[13] Anywhere                   ALLOW IN    172.22.0.0/16

```
Il faut ouvrir le port 3000 pourGrafana, mais cela ne suffit pas si Grafana est installé dans un container.
```
[12] Anywhere                   ALLOW IN    172.21.0.0/16
[13] Anywhere                   ALLOW IN    172.22.0.0/16
```
