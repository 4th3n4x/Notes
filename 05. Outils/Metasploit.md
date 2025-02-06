# Télécharger un exploit

```bash
sudo cp /home/kali/Downloads/50064.rb .
reload_all
```

# Utilisation

```bash
msfconsole
search <name>
use <num>
show options
set / setg (enregistre jusqu'au redémarrage du programme)
```

# Meterpreter

```
upload
download
getuid
shell
hashdump
getsystem
ps
migrate <PID>
```

# Sessions 

```
sessions
sessions -i 1
jobs -l
background
```
# Database

```bash
sudo service postgresql status
sudo systemctl start postgresql
sudo msfdb init
sudo msfdb status
sudo msfdb run
help database
workspace -a Target_1
```