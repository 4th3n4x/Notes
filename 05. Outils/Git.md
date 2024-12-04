# Version

```
git --version
```
# Ajouter à Git

```
cd /home/test/script
git init # initialise le dépôt
git status # vérifié l'état du dépôt
git branch -M main
git add .
git commit -m "Inital commit"
```
# Configurer nom et mail

```
# uniquement pour ce dépôt
git config user.name "Ton Nom"
git config user.email "ton.email@example.com"
git config --list

# globalement
git config --global user.name "Ton Nom"
git config --global user.email "ton.email@example.com"
git config --global --list
```
# Ajouter à Github

Se connecter et créer un dépôt en ligne. Noter l'URL.

```
# lier le dépôt local à celui distant
git remote add origin https://github.com/utilisateur/nom-du-depot.git
# pousser le contenu (main ou master)
git push -u origin main
```

# Ignorer certains fichiers

```
echo "node_modules/" >> .gitignore
echo "*.log" >> .gitignore
```

# Une seule commande

```
git add . && git commit -m "Update scripts" && git push
```