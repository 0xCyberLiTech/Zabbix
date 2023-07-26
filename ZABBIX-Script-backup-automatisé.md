![zabbix-logo](./images/zabbix-logo.png)

# Mise en place d'une sauvegarde automatisée de la base de données Zabbix (MySQL).
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
Récupération des sauvegardes depuis Windows.

Lancer la console CMD en mode administrateur.

Récupération des sauvegardes se trouvant sur la machine distante (Linux /data/zabbix/).

Celles-ci seront déposées sur votre machine Windows dans un dossier que vous aurez créé :

Exemple : D:\MON\DOSSIER\BACKUP
```
 pscp.exe -r -P 2277 root@192.168.50.250:/data/zabbix/* D:\MON\DOSSIER\BACKUP
```
