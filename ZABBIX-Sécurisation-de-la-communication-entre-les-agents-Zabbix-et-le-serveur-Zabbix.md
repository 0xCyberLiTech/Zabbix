<div align="center">

  <br></br>
  
  <a href="https://github.com/0xCyberLiTech">
    <img src="https://readme-typing-svg.herokuapp.com?font=JetBrains+Mono&size=50&duration=6000&pause=1000000000&color=FF0048&center=true&vCenter=true&width=1100&lines=%3ESUPERVISION+ZABBIX_" alt="Titre dynamique SUPERVISION ZABBIX" />
  </a>
  
  <br></br>

  <h2>Laboratoire numérique pour la cybersécurité, Linux & IT.</h2>

  <p align="center">
    <a href="https://0xcyberlitech.github.io/">
      <img src="https://img.shields.io/badge/Portfolio-0xCyberLiTech-181717?logo=github&style=flat-square" alt="🌐 Portfolio" />
    </a>
    <a href="https://github.com/0xCyberLiTech">
      <img src="https://img.shields.io/badge/Profil-GitHub-181717?logo=github&style=flat-square" alt="🔗 Profil GitHub" />
    </a>
    <a href="https://github.com/0xCyberLiTech/Zabbix/releases/latest">
      <img src="https://img.shields.io/github/v/release/0xCyberLiTech/Zabbix?label=version&style=flat-square&color=blue" alt="📦 Dernière version" />
    </a>
    <a href="https://github.com/0xCyberLiTech/Zabbix/blob/main/CHANGELOG.md">
      <img src="https://img.shields.io/badge/📄%20Changelog-Zabbix-blue?style=flat-square" alt="📄 CHANGELOG Zabbix" />
    </a>
    <a href="https://github.com/0xCyberLiTech?tab=repositories">
      <img src="https://img.shields.io/badge/Dépôts-publics-blue?style=flat-square" alt="📂 Dépôts publics" />
    </a>
    <a href="https://github.com/0xCyberLiTech/Zabbix/graphs/contributors">
      <img src="https://img.shields.io/badge/👥%20Contributeurs-cliquez%20ici-007ec6?style=flat-square" alt="👥 Contributeurs Zabbix" />
    </a>
  </p>

</div>

<div align="center">
  <img src="https://img.icons8.com/fluency/96/000000/cyber-security.png" alt="CyberSec" width="80"/>
</div>

<div align="center">
  <p>
    <strong>Cybersécurité</strong> <img src="https://img.icons8.com/color/24/000000/lock--v1.png"/> • <strong>Linux Debian</strong> <img src="https://img.icons8.com/color/24/000000/linux.png"/> • <strong>Sécurité informatique</strong> <img src="https://img.icons8.com/color/24/000000/shield-security.png"/>
  </p>
</div>

---

<div align="center">
  
## À propos & Objectifs.

</div>

Ce projet propose des solutions innovantes et accessibles en cybersécurité, avec une approche centrée sur la simplicité d’utilisation et l’efficacité. Il vise à accompagner les utilisateurs dans la protection de leurs données et systèmes, tout en favorisant l’apprentissage et le partage des connaissances.

Le contenu est structuré, accessible et optimisé SEO pour répondre aux besoins de :
- 🎓 Étudiants : approfondir les connaissances
- 👨‍💻 Professionnels IT : outils et pratiques
- 🖥️ Administrateurs système : sécuriser l’infrastructure
- 🛡️ Experts cybersécurité : ressources techniques
- 🚀 Passionnés du numérique : explorer les bonnes pratiques

---

## ZABBIX - Sécurisation de la communication entre les agents Zabbix et le serveur Zabbix.

👋 Sommaire des sujets abordés :

- 01 - [ZABBIX - Intervention sur une machine Windows, déploiement de la clé PSK sur l'agent zabbix.](#balise-01)
- 02 - [ZABBIX - Intervention sur une machine Linux, déploiement de la clé PSK sur l'agent zabbix.](#balise-02)

<a name="balise-01"></a>
## 01 - Intervention sur une machine Windows, déploiement de la clé PSK sur l'agent zabbix.

Génération de la clé PSK.

Tout d'abord installer le paquet gnutls-bin.
```
apt-get install gnutls-bin
```
Par exemple, générer une clé PSK de 256 bits (32 octets).
```
psktool -u PSK_srv-linux-01 -p databasewindows.psk -s 32
```
Par exemple, générer une clé PSK de 128 bits (16 octets).
```
psktool -u PSK_srv-linux-01 -p databasewindows.psk -s 16
```
Un fichier est créé databasewindows.psk, il faudra y récupérer les informations générées.
```
cat databasewindows.psk
```
```
PSK_srv-linux-01:<CLE-PSK-256-BITS>
```
Sur cette ligne, on retrouve deux données :

- TLSPSKIdentity = PSK_srv-linux-01
- TLSPSKKey = <CLE-PSK-256-BITS>

- Ces deux données seront nécessaires pour paramétrer l'agent Zabbix Windows.
- Ces deux données seront également nécessaires pour déclarer la méthode de chiffrement (PSK) des Hôtes Windows sur le serveur Zabbix.

[Url ou récupérer les agents pour la version de Zabbix 6.4.x](https://www.zabbix.com/fr/download_agents?version=6.0+LTS&release=6.0.3&os=Linux&os_version=4.12&hardware=ppc64le&encryption=No+encryption&packaging=Archive&show_legacy=0)

Nous installons l'agent. (zabbix_agent-6.4.4-windows-amd64-openssl.msi)

Lors de l'installation de l'agent Zabbix pour Windows, effectuer les réglages, voir captures d'écran ci-dessous :

![zabbix-11](./images/zabbix-11.png)

![zabbix-12](./images/zabbix-12.png)

Depuis le serveur Zabbix effectuer les réglages qui s'imposent :

![zabbix-15](./images/zabbix-15.png)

Il faut se rendre sur l'onglet chiffrement et reproduire les mêmes actions :

![zabbix-16](./images/zabbix-16.png)

<a name="balise-02"></a>
## 02 - Intervention sur une machine Linux, déploiement de la clé PSK sur l'agent zabbix.

[Url ou récupérer les agents pour la version de Zabbix 6.4.x](https://www.zabbix.com/fr/download_agents?version=6.0+LTS&release=6.0.3&os=Linux&os_version=4.12&hardware=ppc64le&encryption=No+encryption&packaging=Archive&show_legacy=0)

Nous supposons que l'agent zabbix est déja installé.

Nous devons maintenant modifier le fichier de configuration pour indiquer à l’agent où trouver le serveur.

Ouvrez /etc/zabbix/zabbix_agent2.conf dans votre éditeur de texte préféré et apportez les modifications suivantes pour indiquer à l’agent quels serveurs Zabbix sont autorisés à lui communiquer :
```
nano /etc/zabbix/zabbix_agent2.conf
```
- Server=[IP or hostname of your Zabbix server]
- ServerActive=[IP or hostname of your Zabbix server]

Nous devons également indiquer à Zabbix le nom d’hôte du système.

Il n’est pas nécessaire que ce soit le nom d’hôte réel, c’est le nom d’affichage que nous utiliserons dans Zabbix pour le système.

Commentez la valeur par défaut de Hostname=Zabbix server et remplacez-la par la valeur suivante :
```
HostnameItem=system.hostname
```
```
### Option: Hostname
#       List of comma delimited unique, case sensitive hostnames.
#       Required for active checks and must match hostnames as configured on the server.
#       Value is acquired from HostnameItem if undefined.
#
# Mandatory: no
# Default:
# Hostname=

Hostname=system.hostname
```
Cela indiquera à l’agent de remplir automatiquement la valeur du nom d’hôte avec le nom d’hôte du système.

Vous pouvez simplement définir le nom d’hôte dans le fichier de configuration.

Cependant, le remplissage automatique vous permet de réutiliser le même fichier de configuration sur tous vos hôtes, ce qui simplifie l’automatisation si vous avez beaucoup d’hôtes à surveiller.

Démarrer l’agent 2
```
systemctl enable zabbix-agent2
```
```
systemctl start zabbix-agent2
```
Passons aux clés PSK :

Description :

Par défaut, la communication de l’agent se fait en texte clair.

Pour le cryptage, nous avons la possibilité d’utiliser le cryptage basé sur PSK.

PSK signifie clé pré-partagée.

L’option PSK se compose de deux valeurs importantes, l’identité PSK et le secret PSK.

La clé doit être au minimum axée sur une résolution de 128 bits (PSK de 16 octets, entré sous forme de 32 chiffres hexadécimaux) jusqu’à 2048 bits (PSK de 256 octets, entré sous la forme de 512 chiffres hexadécimaux).

Dans cet exemple, je l’enregistre également directement dans un fichier.

Je crée d’abord un dossier /etc/zabbix/psk_keys/.
```
mkdir -p /etc/zabbix/psk_keys/
```
```
cd /etc/zabbix/psk_keys/
```
Génération de la clé PSK.
```
openssl rand -hex 16 > database.psk
```
Un fichier est créé database.psk.
```
cat database.psk
```
```
<CLE-PSK-128-BITS>
```
Je m’assure également que seul l’utilisateur Zabbix peut lire le fichier.
```
chown zabbix:zabbix database.psk
```
```
chmod 640 database.psk
```
De plus, je reconfigure ensuite le fichier de configuration de l’agent Zabbix.
```
nano /etc/zabbix/zabbix_agent2.conf
```
Se rendre vers la fin du fichier, apporter les modifications suivantes :
```
####### TLS-RELATED PARAMETERS #######

### Option: TLSConnect
#       How the agent should connect to server or proxy. Used for active checks.
#       Only one value can be specified:
#               unencrypted - connect without encryption
#               psk         - connect using TLS and a pre-shared key
#               cert        - connect using TLS and a certificate
#
# Mandatory: yes, if TLS certificate or PSK parameters are defined (even for 'unencrypted' connection)
# Default:
TLSConnect=psk
```
```
### Option: TLSAccept
#       What incoming connections to accept.
#       Multiple values can be specified, separated by comma:
#               unencrypted - accept connections without encryption
#               psk         - accept connections secured with TLS and a pre-shared key
#               cert        - accept connections secured with TLS and a certificate
#
# Mandatory: yes, if TLS certificate or PSK parameters are defined (even for 'unencrypted' connection)
# Default:
TLSAccept=psk
```
```
### Option: TLSPSKFile
#       Full pathname of a file containing the pre-shared key.
#
# Mandatory: no
# Default:
TLSPSKFile=/etc/zabbix/psk_keys/database.psk
```
```
### Option: TLSPSKIdentity
#       Unique, case sensitive string used to identify the pre-shared key.
#
# Mandatory: no
# Default:
TLSPSKIdentity=pskident
```
Récapitulatif :
```
TLSConnect=psk
TLSAccept=psk
TLSPSKFile=/etc/zabbix/psk_keys/database.psk
TLSPSKIdentity=pskident
```
La valeur TLSPSKIdentity que vous décidez de créer ne sera pas chiffrée lors du transport, n’utilisez donc rien de sensible.

Je redémarre ensuite l’agent.
```
systemctl restart zabbix-agent2.service
```
Je vais ensuite dans l’interface utilisateur du serveur Zabbix et configure les options de cryptage PSK pour l’hôte.

De plus, je sélectionne l’icône.

'Connexions à l’hôte' = PSK

'Connexions depuis l’hôte' = PSK

'PSK Identity' = [tout ce que vous avez utilisé dans la configuration de l’agent Zabbix]

'PSK' = [la longue chaîne hexadécimale générée à partir de la commande OpenSSL ci-dessus]

Après une minute ou deux, le serveur et l’agent Zabbix communiqueront avec succès en utilisant le cryptage PSK.

PSK derrière un proxy :

La configuration du chiffrement PSK sur les agents derrière un proxy n’est pas nécessaire s’ils s’exécutent tous sur le même réseau privé interne, sauf si votre stratégie de sécurité recommande également un certain niveau de chiffrement sur vos réseaux internes.

Ce que vous devez faire en premier à la place, c’est activer PSK pour les communications entre votre serveur Zabbix et Zabbix Proxy.

Vous devez créer un nouveau secret, et ajouter l’identité et le secret PSK à Administration ⇾ Proxies ⇾ [Votre proxy] ⇾ Cryptage et également ajuster les paramètres à l’intérieur du fichier de configuration des proxys dans /etc/zabbix/zabbix_proxy.conf.

Avertissement :

Si vous configurez le chiffrement PSK pour les agents derrière un proxy Zabbix, assurez-vous que Zabbix Server ⇽⇾ Proxy PSK est activé en premier. En effet, lorsque vous démarrez le proxy ou effectuez une config_cache_reload, le proxy télécharge tous ses paramètres d’hôte à partir du serveur, ce qui inclut également la copie du secret par le serveur.

Le proxy doit connaître le secret puisqu’il gère maintenant les communications au nom du serveur.

Si vous voulez un cryptage PSK pour tous les agents derrière un proxy, vous continuez à configurer les agents comme d’habitude en créant un nouveau secret, en y éditant la page Configuration ⇾ Hosts ⇾ [Your Host] ⇾ Encryption, et en éditant également leur propre fichier de configuration dans /etc/zabbix/zabbix_agentd.conf.

N’oubliez pas que, puisque la configuration de votre hôte d’agents dans l’interface utilisateur Zabbix sera définie comme surveillée par proxy, les paramètres PSK seront applicables aux communications entre le proxy Zabbix et l’agent qu’il surveille. Pas entre le serveur Zabbix et l’agent derrière le proxy.

Vous pouvez également ajouter le cryptage PSK entre votre proxy Zabbix et son propre agent local si vous le souhaitez. Vous devez définir ses paramètres PSK dans la configuration de l’hôte des agents proxy dans Configuration ⇾ Hôtes ⇾ [Votre proxy] ⇾ Cryptage, et modifier les paramètres dans les agents sur le fichier de configuration dans /etc/zabbix/zabbix_agentd.conf. N’oubliez pas que cela ne s’applique qu’aux communications entre le proxy Zabbix et son propre processus d’agent.

Lors de la configuration du cryptage PSK pour le serveur, le proxy et les agents Zabbix, vous pouvez voir une erreur dans les journaux du proxy,

cannot send proxy data to server at "zabbix.your-domain.tld": connection of type "TLS with PSK" is not allowed for proxy "your-proxy".

Vérifiez d’abord que les paramètres PSK de votre serveur Zabbix ⇽⇾ proxy sont corrects. Ne confondez pas le processus d’agent optionnel des proxys et son processus proxy principal requis.

Dépannage :

Vérifiez les journaux de l’agent Zabbix à l’adresse.
```
tail -f /var/log/zabbix/zabbix_agent2.log
```
Si tout est OK :
```
2023/08/14 22:24:45.639298 Plugin communication protocol version is 6.4.0
2023/08/14 22:24:45.639317 Zabbix Agent2 hostname: [Zabbix server]
```

---

<div align="center">
  <a href="https://github.com/0xCyberLiTech" target="_blank" rel="noopener">
    <img src="https://skillicons.dev/icons?i=linux,debian,bash,docker,nginx,git,vim,python,markdown" alt="Skills" width="440">
  </a>
</div>

<p align="center">
  <b>🔒 Un guide proposé par <a href="https://github.com/0xCyberLiTech">0xCyberLiTech</a> • Pour des tutoriels accessibles à tous. 🔒</b>
</p>

