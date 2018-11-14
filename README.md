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

Endelig skal lave en bruger, og give den adgang til databasen.
```sql
CREATE USER 'name'@'%' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON cbsexam.* TO 'name'@'%';
```

Husk at skifte `name` og `password` ud til noget der giver mere mening :)

## Tomcat
Brug følgende guide til at installere og opsætte tomcat: https://www.digitalocean.com/community/tutorials/how-to-install-apache-tomcat-8-on-centos-7

__DOG SKAL I INSTALLERE JAVA 8, OG IKKE JAVA 7 SOM GUIDEN SIGER__ 

Når det hele er sat op kan i tilgå tomcat på port 8080, med det login som i angav i `tomcat-users.xml`

## Deployment
![peek 2018-11-14 16-39](https://user-images.githubusercontent.com/1210224/48493314-e18aae00-e82b-11e8-91fd-a3ac93033916.gif)
