
![grafana-dashboard-01.png](./images/Grafana-console_01.png)

# ZABBIX - Création de base d'un tableau de bord sur Grafana :

Dans cette section, je vais vous montrer comment créer un tableau de bord Grafana de base en utilisant Zabbix comme source de données pour surveiller la vitesse de téléchargement et de téléchargement réseau du serveur Zabbix.

Pour créer un nouveau tableau de bord Grafana, cliquez sur Tableaux de bord > Gérer à partir de l’interface Web de Grafana.

![grafana-0A](./images/Grafana-console_01.png)

Cliquez sur Nouveau tableau de bord.

![grafana-0B](./images/grafana-0B.png)

Un nouveau tableau de bord doit être créé.

Cliquez sur Ajouter un panneau vide pour ajouter un nouveau panneau au tableau de bord.

![grafana-0C](./images/grafana-0C.png)

L’éditeur de panneau Grafana doit s’afficher. Vous pouvez configurer votre panneau Grafana à partir d’ici.

![grafana-0D](./images/grafana-0D.png)

Tout d’abord, changez la source de données en Zabbix à partir du menu déroulant Source de données comme indiqué dans la capture d’écran ci-dessous.

![grafana-0E](./images/grafana-0E.png)

Pour surveiller la vitesse de téléchargement de votre serveur Zabbix, sélectionnez les paramètres de requête indiqués dans la capture d’écran ci-dessous.

![grafana-0F](./images/grafana-0F.png)

Tapez le titre Vitesse de téléchargement dans la section Titre comme indiqué dans la capture d’écran ci-dessous.

![grafana-0G](./images/grafana-0G.png)

Sélectionnez l’unité Données / bits (IEC) dans la section Unité comme indiqué dans la capture d’écran ci-dessous.

![grafana-0H](./images/grafana-0H.png)

Le graphique de vitesse de téléchargement devrait afficher l’unité de données correcte comme vous pouvez le voir dans la capture d’écran ci-dessous.

![grafana-0I](./images/grafana-0I.png)

Vous pouvez effectuer de nombreuses personnalisations sur votre panneau Grafana. Vous pouvez lire l’article Comment connecter Grafana avec Prometheus? pour en savoir plus.

Une fois que vous êtes satisfait du résultat, cliquez sur Appliquer pour ajouter le panneau au tableau de bord.

![grafana-0J](./images/grafana-0J.png)

Le panneau Vitesse de téléchargement doit être ajouté au tableau de bord.

![grafana-0K](./images/grafana-0K.png)

Maintenant, créons un autre panneau pour surveiller la vitesse de téléchargement du serveur Zabbix.

Comme le panneau de surveillance de la vitesse de téléchargement sera le même que le panneau Vitesse de téléchargement, vous pouvez le cloner et modifier quelques paramètres pour surveiller facilement la vitesse de téléchargement de votre serveur Zabbix.

Pour cloner le panneau Vitesse de téléchargement, cliquez sur la flèche vers le bas du panneau et cliquez sur Plus... > Dupliquer comme indiqué dans la capture d’écran ci-dessous.

![grafana-0L](./images/grafana-0L.png)
