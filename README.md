# Opsætning af cbsexam på AWS
## solr
Den burde i snart have styr på

## MySQL
Lav en ny server i amazon. Security gruppen skal være åben på port 3306

Installer MySQL efter guiden her https://www.digitalocean.com/community/tutorials/how-to-install-mariadb-on-centos-7
I step 3 skal i ændre root passwordet, men resten kan i bare springe over.

Brug `mysql -u root -p` til at oprette databasen: `CREATE DATABASE cbsexam;`.

Derefter downloader i SQL dumpet til serveren `wget https://raw.githubusercontent.com/Distribuerede-Systemer-2018/cbsexam/master/sqldump.sql
`. 

Og indlæser det `mysql -u root -p cbsexam < sqldump.sql`

Endelig skal i sørge for, at root brugeren har lov at forbinde fra andre AWS IP'er. Det gør i med følgende SQL `GRANT ALL PRIVILEGES ON *.* TO 'root'@'172.%'`

## Tomcat
