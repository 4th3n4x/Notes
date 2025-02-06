## Google Dorks

```bash
site:target.com filetype:pdf  # Trouver des PDF sur un site
intitle:"index of" "admin"  # Rechercher des répertoires exposés
inurl:"login"  # Trouver des pages de connexion
site:target.com intext:"password"  # Chercher des mots sensibles
cache:target.com  # Voir la version en cache d'un site
```

## Recherche d'informations sur un domaine

```bash
whois target.com  # Obtenir les informations WHOIS
dig target.com +short  # Trouver l'adresse IP
dig mx target.com  # Trouver les serveurs mail
dig ns target.com  # Trouver les serveurs DNS
```

## Analyse des sous-domaines

```bash
subfinder -d target.com  # Trouver des sous-domaines
assetfinder --subs-only target.com  # Alternative à subfinder
amass enum -d target.com  # Amass pour l'énumération avancée
```

## OSINT sur les réseaux sociaux

```bash
theHarvester -d target.com -b linkedin  # Récupérer des emails sur LinkedIn
sherlock username  # Trouver un pseudo sur plusieurs sites
```

## Recherche d'adresses e-mail

```bash
theHarvester -d target.com -b all  # Récupérer des emails publics
holehe email@domain.com  # Vérifier si un email est utilisé sur des services en ligne
```

## Recherche sur les fuites de données

```bash
dehashed -s email@domain.com  # Vérifier si un email a fuité
```

## Recherche sur les adresses IP

```bash
shodan host 1.2.3.4  # Voir les services exposés d'une IP
```