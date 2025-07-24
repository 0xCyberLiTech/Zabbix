
![grafana-dashboard-01.png](./images/Grafana-panel-01.png)

<div align="center">

<a href="https://github.com/0xCyberLiTech">
  <img src="https://readme-typing-svg.herokuapp.com?font=Fira+Code&size=32&pause=1000&color=D14A4A&center=true&vCenter=true&width=650&lines=SUPERVISION+D'INFRASTRUCTURES;Monitorer+•+Analyser+•+Gérer;Zabbix+•+Nagios+•+Prometheus" alt="Typing SVG" />
</a>

<p align="center">
  <em>Un dépôt pédagogique sur la supervision des infrastructures numériques.</em><br>
  <strong>📊 Monitoring – 📈 Performance – ⚙️ Fiabilité</strong>
</p>

[![Dernière version](https://img.shields.io/github/v/release/0xCyberLiTech/Supervision?style=flat-square&color=blue)](https://github.com/0xCyberLiTech/Supervision/releases/latest)
[![Changelog](https://img.shields.io/badge/📄%20CHANGELOG-Supervision-blue)](https://github.com/0xCyberLiTech/Supervision/blob/main/CHANGELOG.md)
[![Langage](https://img.shields.io/badge/langage-Bash-blue)](https://bash.org/)
[![OS](https://img.shields.io/badge/système-Debian%2012-success)](https://www.debian.org/)
[![Licence](https://img.shields.io/github/license/0xCyberLiTech/Supervision)](LICENSE)
[![Statut](https://img.shields.io/badge/status-en%20développement-orange)]()

</div>

---

### 👨‍💻 **À propos de moi.**

> Bienvenue dans mon **laboratoire numérique personnel** dédié à l’apprentissage et à la vulgarisation de la cybersécurité.  
> Passionné par **Linux**, la **cryptographie** et les **systèmes sécurisés**, je partage ici mes notes, expérimentations et fiches pratiques.  
>  
> 🎯 **Objectif :** proposer un contenu clair, structuré et accessible pour étudiants, curieux et professionnels de l’IT.  
> 🔗 [Mon GitHub principal](https://github.com/0xCyberLiTech)

<p align="center">
  <a href="https://skillicons.dev">
    <img src="https://skillicons.dev/icons?i=linux,debian,bash,docker,nginx,git,vim" alt="Skills" />
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

## ZABBIX - Création de base d'un tableau de bord sur Grafana :

Dans cette section, je vais vous montrer comment créer un tableau de bord Grafana de base en utilisant Zabbix comme source de données pour surveiller la vitesse de téléchargement et de téléchargement réseau du serveur Zabbix.

Pour créer un nouveau tableau de bord Grafana, cliquez sur Tableaux de bord > Gérer à partir de l’interface Web de Grafana.

![grafana-0A](./images/grafana-0A.png)

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

---

**📅 Mise à jour :** Juillet 2025

---

<p align="center">
  <b>🔒 Un guide proposé par <a href="https://github.com/0xCyberLiTech">0xCyberLiTech</a> • Pour une supervision accessible et efficace 🔒</b>
</p>
