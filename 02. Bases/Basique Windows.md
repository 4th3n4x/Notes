# CMD

## Créer un utilisateur et lui attribuer des droits

```cmd
net user user password /add
net localgroup Administrators user /add
net localgroup "Remote Management Users" user /add
```

- **Remote Desktop Users** : Accès RDP
- **Remote Management Users** : Accès WinRM

# Where

```cmd
where ssh
```

# Variables d'environnement (PATH)

```cmd
set PATH=%SystemRoot%\system32;%SystemRoot%;
```

## Autres commandes

```cmd
tree c:\ /f | more
Get-WmiObject -Class win32_OperatingSystem | select Version,BuildNumber
C:\Windows\System32\Config\.
```

# PowerShell

```powershell
powershell -ep bypass
```

### Contourner l'erreur "cannot be loaded because running scripts is disabled"

```powershell
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy Unrestricted
```

## Manipulation de fichiers

```powershell
Add-Content -Path "Chemin_vers_le_fichier" -Value "Contenu à ajouter"
New-Item -Path "Chemin_vers_le_fichier\Nom_du_fichier.extension" -ItemType "file"
```

## Recherche d'une chaîne de caractères

```powershell
Select-String -Path "C:\TEMP\GREP\Data.csv" -Pattern "florian"
```

## Recherche de fichiers

```powershell
Get-ChildItem -Path "C:\" -Recurse -File -include "*motrecherche*"
Get-ChildItem -Path .\*log* -Recurse -Force
```

## Recherche de processus

```powershell
Get-Process | Where-Object {$_.ProcessName -like "*nom*"}
```

## Services en cours d'exécution

```powershell
Get-Service | Where-Object {$_.Status -eq "Running"} | Select-First 2 | Format-List
```

## Droits d'accès

```powershell
Get-ACL -Path HKLM:\System\CurrentControlSet\Services\wuauserv | Format-List
icacls c:\users /grant joe:f
icacls c:\users /remove joe
```

# Autres commandes


```powershell
Get-MpComputerStatus | findstr "True"
```