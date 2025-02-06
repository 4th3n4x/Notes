## Gestion de fichiers

```
with open("fichier.txt", "w") as f:
    f.write("Hello, fichier!")
```

## Serveur

```
import socket
import threading

bind_ip = "0.0.0.0"
bind_port = 7777

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind((bind_ip, bind_port))
server.listen(5)

print("Listening... on %s:%d" % (bind_ip, bind_port))

def client_manage(client_socket):
    while True:
        request = client_socket.recv(1024)
        print ("received")
        client_socket.send("Accepted")

while True:
    client, addr = server.accept()
    print("accepted connection from %s:%d" % (addr[0], addr[1]))
    client_manage = threading.Thread(target=client_manage, args=(client,))
    manage_client.start()
```

## Requêtes

### POST

```
import requests

url = "https://example.com/api"

data = {
    "name": "Alice",
    "age": 25
}

response = requests.post(url, json=data)

if response.status_code == 200:
    print("Données envoyées avec succès :", response.json())
else:
    print("Erreur :", response.status_code, response.text)
```

### GET

```
import requests

url = "https://jsonplaceholder.typicode.com/posts/1"

headers = { "Authorization": "Bearer mon_token" }
response = requests.get(url, headers=headers)

if response.status_code == 200:
    print("Données récupérées avec succès :")
    print(response.json())
else:
    print("Erreur :", response.status_code, response.text)

```

## POO

```
class Personne:
    def __init__(self, nom, age):
        self.nom = nom
        self.age = age

    def se_presenter(self):
        print(f"Je suis {self.nom} et j'ai {self.age} ans.")

p = Personne("Alice", 30)
p.se_presenter()
```

## Gestion erreurs

```
try:
    x = int(input("Entrer un nombre : "))
    print(10 / x)
except ValueError:
    print("Entrée invalide !")
except ZeroDivisionError:
    print("Division par zéro impossible !")
```