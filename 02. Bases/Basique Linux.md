# AWK

```
awk '{print $1}' users.txt
awk -F: '{ print $1 }' user.txt   
```
# CeWL

```
cewl -w list.txt -d 5 -m 5 http://test.com
```

| Arguments | Signification                    |
| --------- | -------------------------------- |
| -d        | Niveau de profondeur du crawling |
| -m        | Nombre de caractères             |

# Cut
| Arguments | Signification |
| ---- | ---- |
| -f | Champ à extraire |
| -c | Caractère à extraire |
| -b | Octet à extraire |
| -d  | Délimiteur |

**Exemples :**
```
cut -d ',' -f 2 test.txt
cut -c 10-20 employes.txt
```

# Echo

```
echo "mon text" > texte.txt
```

- Echapper guillemet : /"
- Ajouter en fin de fichier : >> texte.txt
- Ajouter en début de fichier : ``sed -i '1iLigne1' test.txt``

# Find

```
find / -user margaret -ls 2>/dev/null
find / -type f -user <nom_utilisateur> -exec ls -l {} \;
```

# Grep

```
grep -r "thm"
grep -ri "mot_recherché" /home
```

# SED

```
sed -i 's/$/@runner.htb/' fichier.txt
```

# TR (supprimer \n)

```
tr -d '\n' < test.txt
```

# Vim

```
- i : insert mode
- esc : mode normal
- : : mode commande
- :1 2 aller à la ligne 1
- :w : enregistrer
- :q : quitter
```
# WC

```
wc -c -m -l exemple.txt # Donne tout par défaut
```

| Arguments | Signification |
| ---- | ---- |
| -c | Byte |
| -m | Character |
| -l | Lines |
| -w  | Words |

# SORT

Trie les lignes d'un fichier

# Tuer un processus qui utilise un port

```
sudo netstat -lpn | grep :1337
sudo kill -9 <PID>
```

```
jobs
fg %3
kill -STOP %id
```

# LXC

Télécharger une image dans un répertoire
```
lxc image export ubuntu:16.04 /path/to/download
```