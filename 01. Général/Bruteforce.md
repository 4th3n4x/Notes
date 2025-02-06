# Identifier type de hash

## Hashid

```bash
hashid [-e] [-m] [-j] [-o FILE] [--version] "INPUT"
```

```bash
# -m : hashcat
# -j : john
# -o : sortie dans un fichier
# -e : liste tout
```

# Cracker hash

## Hashcat

```bash
# Trouver le bon numéro pour algorithme correspondant
hashcat -h | grep md5
```

```bash
hashcat -m 1800 "\$6\$aReallyHardSalt\$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02." /usr/share/wordlists/rockyou.txt
```

## JohnTheRipper

```bash
zip2john 8702.zip > hash.txt
rar2john 8703.zip > hash.txt
ssh2john idrsa.rsa > key.txt
```

```bash
john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt --format=[format]
```

```bash
sudo vi /etc/john/john.conf 
[List.Rules:THM-Password-Attacks] 
Az"[0-9]" ^[!@#$]
```

```bash
# | Az        | Mot          |
# | "[0-9]"   | Placée avant |
# | `^[!@#$]` | Placée après |
```

```bash
john --wordlist=/tmp/single.lst --rules=THM-Password-Attacks --stdout 
```

# Cracker mot de passe

## Hydra

```bash
hydra -l user -P passwords.txt <target> http-get-form "/login.php:user=^USER^&pass=^PASS^:F=incorrect" -s 85
hydra -l user -P passwords.txt <target> http-post-form "/login.php:user=^USER^&pass=^PASS^:F=incorrect"
hydra -l user -P passwords.txt ssh://<target>
```

# Générer liste

## Cewl

```bash
cewl -w list.txt -d 5 -m 5 http://test.com
```

```bash
# -d : niveau de profondeur du crawling
# -m : nombre de caractères
```

## Username

```bash
# https://github.com/therodri2/username_generator.git
python3 username_generator.py -h
```

```bash
# https://github.com/urbanadventurer/username-anarchy
```

## Crunch

```bash
crunch 2 2 01234abcd -o crunch.txt
```

## Cupp

```bash
cupp -i
```

# Cewl

```bash
cewl https://www.inlanefreight.com -d 4 -m 6 --lowercase -w inlane.wordlist
```

# Muter liste de password

```bash
hashcat --force password.list -r custom.rule --stdout | sort -u > mut_password.list
```