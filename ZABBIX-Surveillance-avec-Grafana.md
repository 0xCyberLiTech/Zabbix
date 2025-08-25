<div align="center">

  <br></br>
  
  <a href="https://github.com/0xCyberLiTech">
    <img src="https://readme-typing-svg.herokuapp.com?font=JetBrains+Mono&size=50&duration=6000&pause=1000000000&color=FF0048&center=true&vCenter=true&width=1100&lines=%3ESUPERVISION+ZABBIX_" alt="Titre dynamique SUPERVISION ZABBIX" />
  </a>
  
  <br></br>
  
  <p align="center">
    <em>Un dépôt pédagogique sur la supervision des infrastructures numériques.</em><br>
    <strong>📊 Monitoring – 📈 Performance – ⚙️ Fiabilité</strong>
  </p>

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

---

### 👨‍💻 **À propos de moi**

> Bienvenue sur le dépôt <strong>0xCyberLiTech</strong>, votre laboratoire numérique pour l'<strong>apprentissage</strong> et la <strong>vulgarisation</strong> de la <strong>cybersécurité</strong>, de l'<strong>administration Linux Debian</strong> et de la <strong>sécurité informatique</strong>.
> Passionné par <strong>Linux</strong>, la <strong>cryptographie</strong>, la <strong>supervision réseau</strong> et les <strong>systèmes sécurisés</strong>, je partage ici des <strong>tutoriels</strong>, <strong>guides pratiques</strong>, <strong>fiches techniques</strong> et <strong>retours d'expérience</strong> pour renforcer vos compétences IT.
>
> 🎯 <strong>Objectif :</strong> Offrir un contenu structuré, accessible et optimisé pour le référencement naturel, destiné aux étudiants, professionnels, administrateurs système, experts en sécurité et curieux du monde numérique.

<p align="center">
  <a href="https://github.com/0xCyberLiTech" target="_blank" rel="noopener">
    <img src="https://skillicons.dev/icons?i=linux,debian,bash,docker,nginx,git,vim,python,markdown" alt="Skills" width="420">
  </a>
</p>

---

### 🎯 Objectif du dépôt

> Ce dépôt vise à centraliser les connaissances pratiques liées à la **supervision des systèmes d'information**. Il s’adresse à tous ceux souhaitant :
> 
> - Comprendre les enjeux du monitoring
> - Déployer des outils efficaces (Zabbix, Nagios, etc.)
> - Améliorer la **stabilité**, la **performance** et la **disponibilité** de leur infrastructure IT

---

## ZABBIX - Surveillance avec Grafana :

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
[14] 3000/tcp                   ALLOW IN    192.168.0.0/16

```
Il faut ouvrir le port 3000 pourGrafana, mais cela ne suffit pas si Grafana est installé dans un container.
```
[12] Anywhere                   ALLOW IN    172.21.0.0/16 # Docker
[13] Anywhere                   ALLOW IN    172.22.0.0/16 # Docker
```

---

<p align="center">
  <b>🔒 Un guide proposé par <a href="https://github.com/0xCyberLiTech">0xCyberLiTech</a> • Pour des tutoriels accessibles à tous. 🔒</b>
</p>
