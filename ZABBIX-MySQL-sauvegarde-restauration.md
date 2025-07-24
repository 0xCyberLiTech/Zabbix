![zabbix-logo](./images/zabbix-logo.png)

![zabbix-logo](./images/zabbix-logo.png)

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

## Sauvegarde de la base de donnée mysql zabbix :
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
