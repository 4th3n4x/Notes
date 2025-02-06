## Extraction et analyse de fichiers cachés

```bash
binwalk -e <fichier>                # Analyse et extraction automatique
steghide extract -sf <fichier>   # Extraction si mot de passe requis
steghide info <fichier>          # Informations sur l'image stéganographiée
stegseek <fichier>               # Recherche et extraction rapide
zsteg <fichier>                   # Analyse des fichiers PNG pour stéganographie
strings <fichier>                 # Extraction de texte brut caché
stegcrack <fichier> <wordlist>    # Brute-force de mot de passe steghide
```

## Outils avancés

```bash
stegsolve <fichier>               # Analyse visuelle des canaux d'image
java -jar bin/stegsolve.jar        # Lancer Stegsolve
steganabara                        # Interface graphique pour analyse
java -classpath bin steganabara.Steganabara  # Lancer Steganabara
python stegolsb.py extract <fichier>  # Extraction avec stegolsb
```
