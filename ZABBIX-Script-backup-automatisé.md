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

### 👨‍💻 **À propos de moi.**

> Bienvenue dans mon **laboratoire numérique personnel** dédié à l’apprentissage et à la vulgarisation de la cybersécurité.  
> Passionné par **Linux**, la **cryptographie** et les **systèmes sécurisés**, je partage ici mes notes, expérimentations et fiches pratiques.  
> Pproposer un contenu clair, structuré et accessible pour étudiants, curieux et professionnels de l’IT.  

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

## Mise en place d'une sauvegarde automatisée de la base de données Zabbix (MySQL).

```
touch backup-mysql-zabbix.sh
```
```
chmod +x backup-mysql-zabbix.sh
```

Coller le code ci-dessous dans le fichier backup-mysql-zabbix.sh :

```
#! /bin/bash

# --------------------------------------------------------
# 0xCyberLiTech
# Script créé le 23-07-2023
# Script modifié le 23-07-2023
#
# touch backup-mysql-zabbix.sh
# chmod +x backup-mysql-zabbix.sh
# ./backup-mysql-zabbix.sh
# --------------------------------------------------------

path=`dirname $0`
cd "$path"

#Def de variables
user="zabbix"                        # Utilisateur de la base de données
passwd="zabbix"                      # Mot de passe de l'utilisateur de la base
db="zabbix"                          # Nom de la base
dest="/data/zabbix"                  # Chemin de destination de la sauvegarde (Attention, pas de slash à la fin)
nbsav=30                             # Nombre de sauvegardes à conserver

# On fabrique les variables systeme
dte=$(date +"%Y-%m-%d--%H-%M-%S")
fic="$db/$db-$dte.sql"


#### SCRIPT ####

if [ -d $db ]
then
    echo "On sauvegarde dans $db"
else
    echo "On créé le dossier de sauvegarde $db"
    mkdir $db
fi

echo "On sauvegarde de la base $db dans $fic"
ionice -c3 nice -n19 mysqldump -u $user -p$passwd $db > "$fic"

echo "On compresse $fic avec gzip : $fic.gz"
ionice -c3 nice -n19 gzip "$fic"


nbfic=$(ls -C1X "$db" | wc -l)
diff=$(echo $(($nbfic-$nbsav)))

echo "On calcule le nombre de fichiers à supprimer pour ne garder que les $nbsav derniers : $diff"

i=1 # Compteur du for
for f in $(ls -C1X "$db")
do
    if [ $i -le $diff ]
    then
        oldsav="$db/$f"
        echo "On supprime $db/$f"
        rm -f "$db/$f"
        let i++
    else
        echo "Aucune ancienne sauvegarde supprimée"
        break;
    fi
done

echo "On envoie les sauvegarde vers : $dest"
rsync -avzh --delete-after "$db/" "$dest/"
```
Localiser la variable suivante pour modifier la destination de sauvegarde à votre guise.
```
dest="/data/zabbix"                  # Chemin de destination de la sauvegarde (Attention, pas de slash à la fin)
```
```
dest="/ma/destination/zabbix"        # Chemin de destination de la sauvegarde (Attention, pas de slash à la fin)
```
Planifier une tache, une fois par jour à 00 h 00.
```
crontab -e
```
```
00 00 * * * /usr/local/bin/backup-mysql-zabbix.sh
```
Décompresser une archive avec l'extension gz (mon_fichier.gz)
```
cd /data/zabbix
```
```
gzip -d zabbix-2023-07-23--23-14-38.sql.gz
```
```
zabbix-2023-07-23--23-14-38.sql
```
1) Récupération des sauvegardes depuis la machine hôte (Windows) se trouvant sur la machine Zabbix server (distante) dans le but d'un archivage de celles-ci.

Lancer la console CMD en mode administrateur.

Récupération des sauvegardes se trouvant sur la machine distante : (Linux /data/zabbix/).

Celles-ci seront déposées sur votre machine Windows dans un dossier que vous aurez créé : (/data/zabbix).

D:\MON\DOSSIER\BACKUP
```
 pscp.exe -r -P 2277 root@192.168.50.250:/data/zabbix/* D:\MON\DOSSIER\BACKUP
```
2) Envoi de la sauvegarde depuis la machine hôte vers la machine Zabbix server, dans le but d'une restauration :

Récupération de la sauvegarde sur la machine hôte pour l'envoyer sur la machine Zabbix server.

Les archives se trouvent vers D:\DOSSIER_BACKUP sur la machine hôte; (local).
```
 pscp.exe -r -P 2277 D:\DOSSIER_BACKUP\zabbix-2023-08-05--00-00-01.sql.gz root@192.168.50.250:/data/zabbix/
```

---

<p align="center">
  <b>🔒 Un guide proposé par <a href="https://github.com/0xCyberLiTech">0xCyberLiTech</a> • Pour des tutoriels accessibles à tous. 🔒</b>
</p>
