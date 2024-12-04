
# CMD

Créer utilisateur et donner des droits

```
net user user password /add
net localgroup Administrators user /add
net localgroup "Remote Management Users" user /add
```

Remote Desktop Users : rdp
Remote Management Users : winRM

# POWERSHELL

```
powershell -ep bypass
```

## Erreur

*cannot be loaded because running scripts is disabled on this
system*

```
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy Unrestricted
```

Manipulation fichier

```
Add-Content -Path "Chemin_vers_le_fichier" -Value "Contenu à ajouter"
New-Item -Path "Chemin_vers_le_fichier\Nom_du_fichier.extension" -ItemType "file"
```

## Chercher chaîne de caractère

```
Select-String
```
https://www.it-connect.fr/powershell-grep-rechercher-des-chaines-de-caracteres-avec-select-string/

## Chercher fichier

```
Get-ChildItem -Path "C:\" -Recurse -File -include "*motrecherche*"
get-childitem -PATH .\*log* -Recurse -FORCE

Select-String -Path "C:\TEMP\GREP\Data.csv" -Pattern "florian"

-ErrorAction SilentlyContinue 

Get-ChildItem -Path "C:\Temp\GREP\" -Recurse | Select-String -Pattern "florian"

dir C:\flag.txt /s /p /a | findstr /i "flag.txt" 2>nul

```

# Where

```
where ssh
```

# PATH

```
set PATH=%SystemRoot%\system32;%SystemRoot%;
```

# Autres

```
Get-WmiObject -Class win32_OperatingSystem | select Version,BuildNumber

tree
tree c:\ /f | more

icacls c:\users /grant joe:f
icacls c:\users /remove joe

Get-Service | ? {$_.Status -eq "Running"} | select -First 2 |fl
Get-ACL -Path HKLM:\System\CurrentControlSet\Services\wuauserv | Format-List
```

Equivalent help cmd : /?
powershell : Get-Help

```
C:\Windows\System32\Config\.
```

```
Get-MpComputerStatus | findstr "True"
```
