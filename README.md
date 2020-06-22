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


## Creation de la base de donnée


1. Suivre les instructions suivant: ![instruction](documents/instruction.docx)

2. Visualiser vos tables dans pg4Admin




## TIPS Postgresql

1. Liste des user

```cmd
postgres=# \du
```

2. Creation d'un user

```sql
postgres=# CREATE USER [nom user];
```

3. Donner les droits a l'user

```sql
postgres=# ALTER ROLE [nom user] WITH CREATEDB;
```

4. Creation d'une base de donnee

```sql
postgres=# CREATE DATABASE [nom base de donnee] OWNER [nom user];
```


5. Attribuer un mot de passe

```sql
postgres=# ALTER USER [nom user] WITH ENCRYPTED PASSWORD 'mon_mot_de_passe';
```

6. Test

```cmd
postgres=# \q
postgres@linuxlite:~$ psql nom_base_de_donnee
```

7. Infos connexion

```cmd
postgres=# \q
postgres@linuxlite:~$ psql nom_base_de_donnee
```

8. Fichier de configuration

```cmd
root/etc/postgresql/11/main/pg_hba.conf
```

9. Connaitre et changer le Datestyle de la base de donnée

```sql
show datestyle ;
....
ALTER DATABASE [nom de base de donnee] SET Datestyle=iso, mdy;
```