## Directoire

```bash
ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-big.txt -u http://SERVER_IP:PORT/FUZZ
ffuf -w directory.txt:FUZZ1 -w extensions.txt:FUZZ2 -u http://test.com/FUZZ1.FUZZ2
ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-big.txt:FUZZ1 -w /usr/share/seclists/Discovery/Web-Content/web-extensions.txt:FUZZ2 -u http://83.136.252.226:43025/blog/FUZZ1FUZZ2 -v 
```

**Notes** : Vérifier les extensions acceptées avec indexFUZZ

## Récursif

```bash
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ -recursion -recursion-depth 1 -e .php -v
```

## Sous-domaine

```bash
ffuf -w /usr/share/seclists/Discovery/DNS/n0kovo_subdomains.txt -u http://FUZZ.inlanefreight.com
```

## Vhost

```bash
ffuf -w /usr/share/seclists/Discovery/DNS/n0kovo_subdomains.txt -u http://inlanefreight.com -H 'Host: FUZZ.inlanefreight.com'
```

## Paramètres PHP

### GET

```bash
ffuf -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt -u http://site.domaine.local:80/admin/admin.php?FUZZ=key -fw 227
```

### POST

```bash
ffuf -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://site.domaine.local:80/admin/admin.php -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded' -fw 227
```

**Notes** : Essayer avec fs et fw

### Bypass d'une limite

```bash
-H "X-Forwarded-For: W2" -mode pitchfork --rate 100
```