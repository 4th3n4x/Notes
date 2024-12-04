
# Gobuster

```
gobuster dir -u http://<machine_ip> -w <wordlist> -x txt,jpg,php
gobuster dir -u <machine_ip> -w //usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x txt,jpg,php,js,html

gobuster vhost -u http://<machine_ip> -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -t 60 --append-domain
```

```
-x : ajouter extensions
-q : scan silencieux
-o : sortie dans un fichier
-b : exclure erreur 
-k : ne vérifie pas certificat
```

```
gobuster vhost -u <URL to fuzz> -w <wordlist>
```

# DIG

```
dig <MACHINE_IP> //enum serveur DNS
```

# FFUF

```
ffuf -u http://MACHINE_IP/FUZZ -w /usr/share/seclists/Discovery/Web-Content/big.txt
```

```
ffuf -v -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -u http://test.com -t 50 -H "Host: FUZZ.test.com" -fw 3499
```

Note :
- liste à essayer : n0kovo_subdomains.txt

```
ffuf -c -w /usr/share/wordlists/seclists/Discovery/Web-Content/raft-medium-directories.txt -u http://itrc.ssg.htb/?page=FUZZ -b "PHPSESSID=9d8f0d53fe02af65d4058f0777d73103" -recursion -fs 3120
```

# WFUZZ

```
wfuzz -u <http://example.com/?FUZZ=ls+-la> -w <wordlist> --hw 2
wfuzz -b PHPSESSID=619uib13epnc8a9aon4dr325ta -w /usr/share/seclists/Discovery/Web-Content/api/objects.txt --hw 1052 http://admin.holo.live/dashboard.php?FUZZ=id
```

# Sublist3r

```
sublist3r -d example.com -p 80,443
```

# Google Dork

https://gist.github.com/sundowndev/283efaddbcf896ab405488330d1bbc06

# Wois

```
whois test.com -h 192.168.10.10
```

# Scrapy

```
python3 ReconSpider.py <url>
```