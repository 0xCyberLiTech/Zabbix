<div align="center">

<a href="https://github.com/0xCyberLiTech">
  <img src="https://readme-typing-svg.herokuapp.com?font=Fira+Code&size=32&pause=1000&color=33FF33&center=true&vCenter=true&width=750&lines=Tutoriel+:+Installation+Zabbix;Debian+12+•+LAMP+•+Zabbix+7.4;Guide+Étape+par+Étape" alt="Typing SVG" />
</a>

<p align="center">
  <em>Guide complet pour installer Zabbix 7.4 sur Debian 12 avec un environnement LAMP.</em><br>
  <b>📊 Monitoring – 📈 Performance – ⚙️ Fiabilité</b>
</p>

</div>

---

### 🧭 **Sommaire des Étapes**

> **Prérequis :** Avant de poursuivre, assurez-vous d'avoir un système à jour, un serveur Apache2, un serveur MySQL (MariaDB) et un serveur de temps NTP opérationnels.

<div align="center">

| Étape | Description | Accès Rapide |
|:---:|:---|:---:|
| **01** | Installer un serveur Apache2 fonctionnel de base | [<img src="https://img.shields.io/badge/VOIR-brightgreen?style=for-the-badge&logo=apache&logoColor=white">](#étape-01) |
| **02** | Créer deux VirtualHosts (HTTP & HTTPS) | [<img src="https://img.shields.io/badge/EXPLORER-brightgreen?style=for-the-badge&logo=github&logoColor=white">](https://github.com/0xCyberLiTech/Apache2/blob/main/Cr%C3%A9%C3%A9-deux-VirtualHosts-HTTP-HTTPS.md) |
| **03** | Installer PHP & PHP-FPM | [<img src="https://img.shields.io/badge/VOIR-brightgreen?style=for-the-badge&logo=php&logoColor=white">](#étape-03) |
| **04** | Installer et sécuriser le serveur MariaDB (MySQL) | [<img src="https://img.shields.io/badge/VOIR-brightgreen?style=for-the-badge&logo=mariadb&logoColor=white">](#étape-04) |
| **05** | Installer Zabbix Server, Frontend & Agent 2 (v7.4) | [<img src="https://img.shields.io/badge/VOIR-brightgreen?style=for-the-badge&logo=zabbix&logoColor=white">](#étape-05) |
| **06** | Installer et configurer l'Agent Zabbix 2 (Client) | [<img src="https://img.shields.io/badge/VOIR-brightgreen?style=for-the-badge&logo=zabbix&logoColor=white">](#étape-06) |
| **07** | Infos complémentaires (Firewall, Erreurs, Scripts) | [<img src="https://img.shields.io/badge/VOIR-blue?style=for-the-badge&logo=readme&logoColor=white">](#étape-07) |

</div>

---

<a name="étape-01"></a>
### ✅ Étape 1 : Installer un serveur Apache2

```bash
apt -y install apache2
systemctl start apache2.service
systemctl enable apache2.service
systemctl status apache2.service
