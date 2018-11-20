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
Lav en ny server i amazon. Security gruppen skal være åben på port 8080

Brug følgende guide til at installere og opsætte tomcat: https://www.digitalocean.com/community/tutorials/how-to-install-apache-tomcat-8-on-centos-7
Brug `wget http://mirrors.dotsrc.org/apache/tomcat/tomcat-8/v8.5.35/bin/apache-tomcat-8.5.35.tar.gz` til at downloade tomcat (linket i guiden virker ikke)

__DOG SKAL I INSTALLERE JAVA 8, OG IKKE JAVA 7 SOM GUIDEN SIGER__ 

Når det hele er sat op kan i tilgå tomcat på port 8080, med det login som i angav i `tomcat-users.xml`

## Deployment
Først skal i tilføje et nyt artifakt til projektet, som bygger jeres kode som en .war fil:

![peek 2018-11-14 16-39](https://user-images.githubusercontent.com/1210224/48493314-e18aae00-e82b-11e8-91fd-a3ac93033916.gif)

Husk at ændre i config.json så det passer til jeres mysql og solr servere på amazon. Husk at bruge interne IP'er.

Hver gang i vil deploye en ny version af koden skal i bygge war filen igen via Build -> Build Artifacts -> cbsexam:war -> Build.

For at deploye koden går i ind i tomcat manageren på `http://[tomcat ip]:8080/manager` og logger ind med det bruger og password i skrev i tomcat-users.xml.

Under 'WAR file to deploy' uploader i jeres war fil. Derefter kan i tilgå programmet på `http://[tomcat ip]:8080/cbsexam_war`.

For at uploade en ny version af koden skal i først vælge 'Undeploy' under 'applications', og defefter kan i uploade den nye .war fil.
