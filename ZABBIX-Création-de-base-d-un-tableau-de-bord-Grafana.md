<div align="center">

  <br></br>
  
  <a href="https://github.com/0xCyberLiTech">
    <img src="https://readme-typing-svg.herokuapp.com?font=JetBrains+Mono&size=50&duration=6000&pause=1000000000&color=FF0048&center=true&vCenter=true&width=1100&lines=%3ESUPERVISION+ZABBIX_" alt="Titre dynamique SUPERVISION ZABBIX" />
  </a>
  
  <br></br>

  <h2>Laboratoire numérique pour la cybersécurité, Linux & IT</h2>

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

<!--
Optimisation SEO : mots-clés Zabbix, 0xCyberLiTech, supervision informatique, monitoring, Zabbix, administration système, sécurité informatique, Linux, Debian, tutoriels supervision, guides monitoring, alertes réseau, performance réseau, open source, ressources techniques, IT, professionnels, étudiants, passionnés, gestion d’infrastructure, surveillance réseau, outils de supervision.
-->

<div align="center">
  <img src="https://img.icons8.com/fluency/96/000000/cyber-security.png" alt="CyberSec" width="80"/>
</div>

<div align="center">
  <p>
    <strong>Cybersécurité</strong> <img src="https://img.icons8.com/color/24/000000/lock--v1.png"/> • <strong>Linux Debian</strong> <img src="https://img.icons8.com/color/24/000000/linux.png"/> • <strong>Sécurité informatique</strong> <img src="https://img.icons8.com/color/24/000000/shield-security.png"/>
  </p>
</div>

---

## 🚀 À propos & Objectifs

Ce projet propose des solutions innovantes et accessibles en cybersécurité, avec une approche centrée sur la simplicité d’utilisation et l’efficacité. Il vise à accompagner les utilisateurs dans la protection de leurs données et systèmes, tout en favorisant l’apprentissage et le partage des connaissances.

Le contenu est structuré, accessible et optimisé SEO pour répondre aux besoins de :
- 🎓 Étudiants : approfondir les connaissances
- 👨‍💻 Professionnels IT : outils et pratiques
- 🖥️ Administrateurs système : sécuriser l’infrastructure
- 🛡️ Experts cybersécurité : ressources techniques
- 🚀 Passionnés du numérique : explorer les bonnes pratiques

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

![grafana-dashboard-01.png](./images/Grafana-panel-01.png)

---

<div align="center">
  <a href="https://github.com/0xCyberLiTech" target="_blank" rel="noopener">
    <img src="https://skillicons.dev/icons?i=linux,debian,bash,docker,nginx,git,vim,python,markdown" alt="Skills" width="440">
  </a>
</div>

<p align="center">
  <b>🔒 Un guide proposé par <a href="https://github.com/0xCyberLiTech">0xCyberLiTech</a> • Pour des tutoriels accessibles à tous. 🔒</b>
</p>
