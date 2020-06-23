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


![pg4Admin](documents/pg4admin_tables.png)


## Visualiser le contenu de vos tables dans pg4Admin

#### SELECT

Exemple: Interroger tous le contenu de la table performance:

```sql
SELECT * FROM performance
```

![pg4Admin_perf](documents/pg4admin_tables_perf.png)


Exemple: Interroger le contenu des colonnes mkt_carrier, mkt_carrier_fl_num, origine  de la table performance:

```sql
SELECT mkt_carrier, mkt_carrier_fl_num, origin FROM performance
```

![pg4Admin_perf](documents/pg4admin_tables_perf2.png)

Exemple: Interroger le contenu des colonnes mkt_carrier, mkt_carrier_fl_num, origin renomme de la table performance: 

```sql
SELECT mkt_carrier AS airline, mkt_carrier_fl_num AS flight, origin AS depart_city FROM performance
```

![pg4Admin_perf](documents/pg4admin_tables_perf3.png)

#### SELECT DISTINCT

Exemple: Interroger le contenu de la colonne mkt_carrier de la table performance en retirant les doublons

```sql
SELECT DISTINCT mkt_carrier FROM performance
```

![pg4Admin_perf](documents/pg4admin_tables_perf4.png)


Exemple: Interroger le contenu de la colonne mkt_carrier  et de la colonne origin renomme de la table performance en retirant les doublons

```sql
SELECT DISTINCT mkt_carrier, origin AS depart_city FROM performance
```

![pg4Admin_perf](documents/pg4admin_tables_perf5.png)

## Limiter le nombre de resultat

#### Clause WHERE

Exemple: Interroger le contenu de la colonne mkt_carrier  et de la colonne origin renomme de la table performance en retirant les doublons lorsque mkt_carrier='UA'

```sql
SELECT DISTINCT mkt_carrier, origin AS depart_city FROM performance WHERE mkt_carrier='UA'
```

![pg4Admin_perf](documents/pg4admin_tables_perf6.png)


#### Clause WHERE avec operateur de comparaison (et, ou, >, <, =, <>,>= et <=)

Exemple: Interroger le contenu de la colonne mkt_carrier  et de la colonne origin renomme de la table performance lorsque mkt_carrier='UA' et depart_city='BZN'

```sql
SELECT mkt_carrier, origin AS depart_city FROM performance WHERE mkt_carrier='UA' AND origin='BZN'
```

![pg4Admin_perf](documents/pg4admin_tables_perf7.png)

#### Matching patterns (LIKE)

Le pattern peut prendre un caratere generique avec %.

Oubien nous cherchons le terme exact avec _.

Exemple: Interroge notre table performance lorsque origin_city_name match avec 'Fort%' (plus qqchose).

```sql
SELECT 	fl_date,
		mkt_carrier AS airline,
		mkt_carrier_fl_num AS flight,
		origin_city_name 
	FROM performance
WHERE origin_city_name LIKE 'Fort%';
```

![pg4Admin_perf](documents/pg4admin_tables_perf8.png)

Exemple: Interroge notre table performance lorsque origin_city_name match avec 'New%LA' en retirant les doublons.

```sql
SELECT DISTINCT	origin_city_name 
	FROM performance
WHERE origin_city_name LIKE 'New%LA';
```

![pg4Admin_perf](documents/pg4admin_tables_perf9.png)

Exemple: Interroge notre table performance lorsque origin_city_name match avec '____,KS'.

```sql
SELECT DISTINCT	origin_city_name 
	FROM performance
WHERE origin_city_name LIKE '____, KS';
```

![pg4Admin_perf](documents/pg4admin_tables_perf10.png)

Exemple: Interroge notre table performance lorsque origin_city_name match avec '____,%'.

```sql
SELECT DISTINCT	origin_city_name 
	FROM performance
WHERE origin_city_name LIKE '____, %';
```

![pg4Admin_perf](documents/pg4admin_tables_perf11.png)

*A l'inverser nous pouvons utiliser la clause WHERE...NOT LIKE*


#### IS NULL et IS NOT NULL

Le contenu d'un champs retourne NULL lorsque:

* celui-ci est abscent, sql reconnaît un champ vide comme une valeur nulle
* lors d'un calcule faux (par exemple x/0)
* l'utilisation d'un caractère d'espace vide est une mauvaise pratique

Exemple: Interroge notre table performance lorsque cancellation_code IS NOT NULL.

```sql
SELECT	fl_date,
		mkt_carrier AS airline,
		mkt_carrier_fl_num AS flight,
		cancellation_code
	FROM performance
WHERE cancellation_code IS NOT NULL;

```

![pg4Admin_perf](documents/pg4admin_tables_perf12.png)

Exemple: Interroge notre table performance lorsque cancellation_code IS NULL.

```sql
SELECT	fl_date,
		mkt_carrier AS airline,
		mkt_carrier_fl_num AS flight,
		cancellation_code
	FROM performance
WHERE cancellation_code IS NULL;

```

![pg4Admin_perf](documents/pg4admin_tables_perf13.png)

#### Combinaisons (BETWEEN et IN)

Exemple: Interroge une table pour retourner les resultats compris entre deux valeur (encadrement).

```sql
SELECT	first_name,
		age
	FROM person
WHERE age >=19
	AND age <=35;
```

Et equivalent à :

```sql
SELECT	first_name,
		age
	FROM person
WHERE age BETWEEN 19 and 35;
```

Exemple: Interroge une table pour retourner les resultats avec une clause de multiple OR.

```sql
SELECT	first_name,
		age
	FROM person
WHERE first_name = "Jimmy"
	OR first_name = "Brenna"
	OR first_name = "Elmo";
```

Et equivalent à :

```sql
SELECT	first_name,
		age
	FROM person
WHERE first_name IN 
	("Jimmy, Brenna, Elmo")
```

Inversement:

```sql
SELECT	first_name,
		age
	FROM person
WHERE first_name NOT IN 
	("Jimmy, Brenna, Elmo")
```

#### Ordre dde priorite des operations

* AND a une priorité d'opérateur plus élevée que OR

Exemple:

![pg4Admin_perf](documents/pg4admin_tables_perf14.png)

* Le contenu compris entre () est evalue avant le reste.

Exemple:

![pg4Admin_perf](documents/pg4admin_tables_perf15.png)

## Utilisation de JOIN

* L'utilisation de JOIN permet de recuperer une combinaison d'enregistrement et de datas issues de plusieurs tables.

* JOIN permet d'avoir une database relationnelle.
* 3 types de JOIN exist (INNER, OUTER et FULL JOIN)

#### Data Key

* Les cles sont des champs qui décrivent les relations entre les tables
* Une cle primaire (Primary key) sont unique pour chaque enrtegistrement dans la table
* Une cle etrangere d'une table fait reference à la cle primaire d'une autre table 

#### INNER JOIN

![pg4Admin_perf](documents/pg4admin_tables_perf16.png)

* Renvoie toutes les lignes de deux tables ou plus qui remplissent la condition de jointure
* Les champs joints doivent exister dans les deux tables

Exemple:

![pg4Admin_perf](documents/pg4admin_tables_perf17.png)

Resultat:

![pg4Admin_perf](documents/pg4admin_tables_perf18.png)

Exemple avec limitation des resultats:

![pg4Admin_perf](documents/pg4admin_tables_perf19.png)

Exemple avec limitation des resultats avec clause where:

![pg4Admin_perf](documents/pg4admin_tables_perf20.png)

Syntaxe alternative:

![pg4Admin_perf](documents/pg4admin_tables_perf21.png)

#### Utilisation d'alias

Exemple:

![pg4Admin_perf](documents/pg4admin_tables_perf22.png)

Par convention les alias sont notés telque:

![pg4Admin_perf](documents/pg4admin_tables_perf23.png)

L'alias deviens implicite et le mot cle AS disparait.

#### OUTER JOIN

![pg4Admin_perf](documents/pg4admin_tables_perf24.png)

Syntaxes:

LEFT OUTER JOIN ---> equivalent a ---> LEFT JOIN

RIGHT OUTER JOIN ---> equivalent a ---> RIGHT JOIN

Exemple LEFT OUTER JOIN:

```sql
SELECT	c.first_name,
		c.last_name,
		o.order_date,
		o.order_amount
	FROM customers c
LEFT OUTER JOIN orders o
		ON c.customers_id = o.customers_id
```

Resultat:

![pg4Admin_perf](documents/pg4admin_tables_perf25.png)


Exemple RIGHT OUTER JOIN:
```sql
SELECT	c.first_name,
		c.last_name,
		o.order_date,
		o.order_amount
	FROM customers c
RIGHT OUTER JOIN orders o
		ON c.customers_id = o.customers_id
```

Resultat:

![pg4Admin_perf](documents/pg4admin_tables_perf26.png)


LEFT OUTER JOIN sont beaucoup plus répandues dans la pratique.

Peut être plus facile à lire et à interpréter.


#### FULL JOIN

![pg4Admin_perf](documents/pg4admin_tables_perf27.png)

* Renvoie toutes les lignes de deux tables ou plus, que la condition de jointure soit remplie ou non.
* Si aucune correspondance, le côté manquant contiendra null

Exemple FULL OUTER JOIN:

```sql
SELECT	c.first_name,
		c.last_name,
		o.order_date,
		o.order_amount
	FROM customers c
FULL OUTER JOIN orders o
		ON c.customers_id = o.customers_id
```

Resultat:

Le mot clé FULL OUTER JOIN spécifie le type de jointure ON les champs à joindre y compris les enregistrements sans correspondance (null).


![pg4Admin_perf](documents/pg4admin_tables_perf28.png)

#### Rechercher dans les tables (LOOKUP)

Exemple: Dans cet exemple, nous allons utiliser une table de correspondance (LOOKUP TABLE) pour convertir nos codes d'operateur de marketing en noms de compagnie aeriennes.

```sql
SELECT 	p.fl_date,
		p.mkt_carrier,
		cc.carrier_desc AS airline,
		p.mkt_carrier_fl_num AS flight,
		p.origin_city_name,
		p.dest_city_name,
		p.cancellation_code,
		ca.cancel_desc
	FROM performance p
		INNER JOIN codes_carrier cc 
			ON p.mkt_carrier = cc.carrier_code
		LEFT JOIN codes_cancellation ca
			ON p.cancellation_code = ca.cancellation_code;
```

Utiliser une clause WHERE p.cancellation_code IS NOT NULL, pour limiter les resultats.


![pg4Admin_perf](documents/pg4admin_tables_perf29.png)

Recap diagram de Venn:

![pg4Admin_perf](documents/pg4admin_tables_perf30.png)

https://en.wikipedia.org/wiki/Venn_diagram


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