
# Identifier type de hash

## Hashid

```
hashid [-e] [-m] [-j] [-o FILE] [--version] "INPUT"
```

```
-m : hashcat
-j : john
-o : sortie dans un fichier
-e : liste tout
```

# Cracker hash

## Hashcat

Trouver le bon numéro pour algorithme correspondant

```
hashcat -h | grep md5
```

```
hashcat -m 1800 "\$6\$aReallyHardSalt\$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02." /usr/share/wordlists/rockyou.txt
```

## JohnTheRipper

```
zip2john 8702.zip > hash.txt
rar2john 8703.zip > hash.txt
ssh2john idrsa.rsa > key.txt
```

```
john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt --format=[format]
```

```
sudo vi /etc/john/john.conf 
[List.Rules:THM-Password-Attacks] 
Az"[0-9]" ^[!@#$]
```

```
| Az        | Mot          |
| "[0-9]"   | Placée avant |
| `^[!@#$]` | Placée après |
```

```
john --wordlist=/tmp/single.lst --rules=THM-Password-Attacks --stdout 
```

# Cracker mot de passe

## Hydra

```
hydra -l user -P passwords.txt <target> http-get-form "/login.php:user=^USER^&pass=^PASS^:F=incorrect" -s 85
hydra -l user -P passwords.txt <target> http-post-form "/login.php:user=^USER^&pass=^PASS^:F=incorrect"
hydra -l user -P passwords.txt ssh://<target>
```

# Générer liste

## Cewl

```
cewl -w list.txt -d 5 -m 5 http://test.com
```

```
-d : niveau de profondeur du crawling
-m : nombre de caractères
```

## Username

https://github.com/therodri2/username_generator.git
```
python3 username_generator.py -h
```

https://github.com/urbanadventurer/username-anarchy

## Crunch

```
crunch 2 2 01234abcd -o crunch.txt
```

## Cupp

```
cupp -i
```

# Cewl

```
cewl https://www.inlanefreight.com -d 4 -m 6 --lowercase -w inlane.wordlist
```

# Muter liste de password

```
hashcat --force password.list -r custom.rule --stdout | sort -u > mut_password.list
```