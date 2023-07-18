![zabbix-logo](./images/zabbix-logo.png)
# Installation de ZABBIX à l'aide docker compose.

## Sommaire :

| Cat | Etapes |
|------|------| 
| - A. | [Cest quoi ZABBIX ?](#balise_01) |
| - a1. | [Structure du logiciel.](#balise_02) |
| - a2. | [Le serveur de données.](#balise_03) |
| - a3. | [L'interface de gestion.](#balise_04) |
| - a4. | [Le serveur de traitement.](#balise_05) |
| - a5. | [Méthode de traitement.](#balise_06) |
| - a6. | [Items.](#balise_07) |
| - a7. | [Triggers.](#balise_08) |
| - a8. | [Action.](#balise_09) |
| - B. | [Installation de ZABBIX à l'aide de docker compose v2.](install-zabbix-doker-compose.md) |
| - C. | [Comment installer l'agent Zabbix sur Debian 12.](Installer_l_agent_Zabbix_sur_Debian_12.md) |



<a name="balise_01"></a>
# - A. Cest quoi ZABBIX ?

ZABBIX est un logiciel libre permettant de surveiller l'état de divers services réseau, serveurs et autres matériels réseau et produisant des graphiques dynamiques de consommation des ressources.

C'est un logiciel créé par Alexei Vladishev.

<a name="balise_02"></a>
## - a1. Structure du logiciel.

Le « serveur ZABBIX » peut être décomposé en trois parties séparées : 

- Le serveur de données.
- L'interface de gestion.
- Le serveur de traitement. 

Chacune d'elles peut être disposée sur une machine différente pour répartir la charge et optimiser les performances.

Le système dont l'utilisation des ressources doit être analysée comporte un agent fonctionnant sous forme de daemon appelé zabbix-agentd et écoutant par défaut sur le port TCP 10050. 

Celui-ci intègre des fonctions permettant d'échantillonner l'état des ressources des différents composants du système (Mémoire, CPU, débit réseau, entrées-sorties, nombre de connexion à une application, etc.) et propose si nécessaire l'exécution de scripts. Le serveur Zabbix appelle donc régulièrement cet agent et lui demande les informations concernant telle ou telle ressource.

<a name="balise_03"></a>
## - a2. Le serveur de données.

ZABBIX utilise MySQL, PostgreSQL ou Oracle pour stocker les données. 

Selon l'importance du nombre de machines et de données à surveiller, le choix du SGBD influe grandement sur les performances. Il existe une section relative à ce choix dans le manuel officiel. A savoir que l'éditeur développe en premier lieu sur l'écosystème MySQL (MariaDB, Percona, ...).

<a name="balise_04"></a>
## - a3. L'interface de gestion.

Son interface web est écrite en PHP. Elle agit directement sur les informations stockées dans la base de données. Chaque information nécessaire au serveur de traitement étant réactualisée automatiquement, il n'y a pas d'action à effectuer sur le binaire pour lui indiquer qu'il y a eu une mise à jour.

Cette interface dispose des fonctionnalités principales suivantes :

- Affichage des données et état des machines
- Génération de graphiques (évolution des données et état des machines/liens)
- Classement et groupement des machines surveillées
- Auto découverte de machines et ajout automatique
- Gestion fine des droits d'accès pour les utilisateurs de l'interface

<a name="balise_05"></a>
## - a4. Le serveur de traitement.

Il s'agit d'un démon binaire existant pour Linux, BSD et divers Unix (voir site officiel : http://www.zabbix.com/requirements.php). 

Il offre diverses options de monitoring. La vérification simple permet de vérifier la disponibilité ainsi que le temps de réponse de services standards comme SMTP ou HTTP sans installer aucun logiciel sur l'hôte monitoré. Un agent ZABBIX peut aussi être installé sur les hôtes Linux, UNIX et Windows afin d'obtenir des statistiques comme la charge CPU, l'utilisation du réseau, l'espace disque... Le logiciel peut réaliser le monitoring via SNMP.

Fonctionnalité intéressante, il est possible de configurer des "proxy Zabbix" afin de répartir la charge ou d'assurer une meilleure disponibilité de service.

Le logiciel Zabbix est écrit en langage C.

<a name="balise_06"></a>
## - a5. Méthode de traitement.

Pour ZABBIX, chaque valeur récupérée correspond à un item. A chacun d'eux peut être associé un ou plusieurs tests appelés triggers. Des actions peuvent être liées aux triggers, ce qui permet d'effectuer un traitement particulier (notification, remédiation, ...) pour chaque anomalie pouvant survenir. Par exemple, si une machine devient indisponible, on peut envoyer un mail à l'administrateur système. Si la charge d'un programme devient trop importante pendant trop longtemps, on peut lancer un programme qui fera un flush...

- Collecter l'information est donc le premier traitement réalisé (on peut adjoindre à cette collecte un premier niveau de transformation de la donnée collectée).
- Stocker cette information dans une base de données est le deuxième traitement.
- Analyser les conditions de déclenchement d'un événement sera la troisième étape.
- Restituer les événements, mais également les indicateurs collectés sous forme de graphe dans le temps, sera réalisé par le frontal Web.

<a name="balise_07"></a>
## - a6. Items.

Les items sont des valeurs récupérées par le serveur ZABBIX. Leur source peut être sélectionnée. Elles peuvent être des réponses ou trap SNMP, des codes de retour ou le résultat de programmes externes, des valeurs demandées à un agent ZABBIX, des compteurs JMX, des valeurs calculées (formule mathématique de plusieurs indicateurs bruts), des valeurs agrégées (agrégation d'une valeur collectée pour un groupe d'équipements), ...

Pour chaque item, on peut spécifier la durée d'enregistrement dans la base de chaque valeur remontée.

<a name="balise_08"></a>
## - a7. Triggers.

Les triggers sont des tests effectués sur un ou plusieurs item. Ils peuvent avoir des dépendances. Cela permet d'éviter de générer des alertes pour des machines si c'est le réseau en amont qui est défaillant. Les triggers représentent la fonction d'analyse des conditions de déclenchement d'un événement. Étant donné que cette analyse se fait sur les données collectées, on peut baser notre analyse sur un ou plusieurs indicateurs, en provenance d'un ou plusieurs équipements : il s'agit de fonction de corrélation.

<a name="balise_09"></a>
## - a8. Action.

Une action peut être lancée sur 4 types d'événements : 

les événements de découverte, les événements d'auto-enregistrement des agents, les événements internes et les triggers. Dans ce dernier cas, on pourra définir des actions de notification (envoi de courriel, d'événement sur messagerie instantanée, création de ticket d'incident...) et des actions de remédiation (tentative de correction automatisée de l'anomalie).

Les actions permettent de concevoir des scénarios d'escalade, comme :

- l'envoi immédiat d'un courriel à un administrateur du composant touché par l'anomalie.
- l'envoi d'un SMS 5 min après le déclenchement de l'événement à un administrateur du composant touché par l'anomalie, si celle-ci perdure pendant ce laps de temps.
- la création d'un ticket d'incident 10 min après le déclenchement de l'événement, si l'anomalie perdure pendant ce temps.
- etc.

Source : https://fr.wikipedia.org/w/index.php?title=Zabbix&action=edit&section=1
