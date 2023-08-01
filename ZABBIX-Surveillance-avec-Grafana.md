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
