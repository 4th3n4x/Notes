## Capture

```bash
sudo tcpdump -i ens5 -c 5 -n
```

### Commandes

|Command|Explication|
|---|---|
|`tcpdump -i INTERFACE`|Capture les paquets sur une interface réseau spécifique|
|`tcpdump -w FILE`|Enregistre les paquets capturés dans un fichier|
|`tcpdump -r FILE`|Lit les paquets capturés à partir d'un fichier|
|`tcpdump -c COUNT`|Capture un nombre spécifique de paquets|
|`tcpdump -n`|Ne résout pas les adresses IP|
|`tcpdump -nn`|Ne résout ni les adresses IP ni les numéros de protocole|
|`tcpdump -v`|Affichage verbeux; peut être augmenté avec `-vv` et `-vvv`|

## Filtrage

```bash
sudo tcpdump host example.com -w http.pcap
sudo tcpdump -i ens5 port 53 -n
sudo tcpdump -i ens5 icmp -n
```

### Options de filtrage

|Commande|Explication|
|---|---|
|`tcpdump host IP` ou `tcpdump host HOSTNAME`|Filtre les paquets par adresse IP ou nom d'hôte|
|`tcpdump src host IP`|Filtre les paquets par une source spécifique|
|`tcpdump dst host IP`|Filtre les paquets par une destination spécifique|
|`tcpdump port PORT_NUMBER`|Filtre les paquets par numéro de port|
|`tcpdump src port PORT_NUMBER`|Filtre les paquets par le port source spécifié|
|`tcpdump dst port PORT_NUMBER`|Filtre les paquets par le port destination spécifié|
|`tcpdump PROTOCOL`|Filtre les paquets par protocole (ex: ip, ip6, icmp)|

## Lecture

```bash
tcpdump -r traffic.pcap src host 192.168.124.1 -n | wc
sudo tcpdump -r traffic.pcap arp and host 192.168.124.137
```

## Affichage des paquets

|Commande|Explication|
|---|---|
|`tcpdump -q`|Affichage rapide et concis des paquets|
|`tcpdump -e`|Inclut les adresses MAC|
|`tcpdump -A`|Affiche les paquets en encodage ASCII|
|`tcpdump -xx`|Affiche les paquets en format hexadécimal|
|`tcpdump -X`|Affiche les paquets en format hexadécimal et ASCII|