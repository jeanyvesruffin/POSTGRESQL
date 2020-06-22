# POSTGRESQL

## Installation prealable

1. https://www.postgresql.org/download/linux/ubuntu/

2. https://wiki.postgresql.org/wiki/Apt

```cmd
Succès. Vous pouvez maintenant lancer le serveur de bases de données en utilisant :

    pg_ctlcluster 11 main start

```

3. Verifier version postgresql

```cmd
psql --version
```

4. Demarrage postgres

```cmd
sudo -i -u postgres
pg_ctlcluster 11 main start
sudo -i -u postgres
psql
```

5. Redemarrage service postgreql

```cmd
sudo service postgresql restart
```

6. Suivre les recommandation §3.7 https://doc.ubuntu-fr.org/postgresql


7. Creer la base de donnee

Suivre les instructions suivant: file://documents/How to create demonstration tables.docx


## Creation de la base de donnée


1. Telecharger le dataset de https://www.transtats.bts.gov/tables.asp?DB_ID=120.

2. 


## TIPS Postgresql

* Liste des user

```cmd
postgres=# \du
```

* Creation d'un user

```cmd
postgres=# CREATE USER [nom user];
```

* Donner les droits a l'user

```cmd
postgres=# ALTER ROLE [nom user] WITH CREATEDB;
```

* Creation d'une base de donnee

```cmd
postgres=# CREATE DATABASE [nom base de donnee] OWNER [nom user];
```


* Attribuer un mot de passe

```cmd
postgres=# ALTER USER [nom user] WITH ENCRYPTED PASSWORD 'mon_mot_de_passe';
```

* Test

```cmd
postgres=# \q
postgres@linuxlite:~$ psql nom_base_de_donnee
```

* Infos connexion

```cmd
postgres=# \q
postgres@linuxlite:~$ psql nom_base_de_donnee
```

* Fichier de configuration

```cmd
root/etc/postgresql/11/main/pg_hba.conf
```
