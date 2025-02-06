```
# Exécution d'un script
./script.sh  # Exécute un script (nécessite chmod +x)
bash script.sh  # Exécute avec Bash directement

# Variables
VAR="valeur"
echo $VAR  # Affiche la valeur
echo "${VAR}"  # Bonne pratique

# Lecture d'entrée
read VAR  # Stocke l'entrée utilisateur dans VAR

# Conditions
if [ "$VAR" = "test" ]; then
    echo "Match!"
fi

# Boucles
for i in {1..5}; do echo "Itération $i"; done

while [ "$VAR" != "quit" ]; do
    read VAR
    echo "Vous avez tapé : $VAR"
done

# Fonctions
ma_fonction() {
    echo "Hello, $1"
}
ma_fonction "World"

# Opérations sur fichiers
ls -l fichier.txt  # Liste détaillée
rm fichier.txt  # Supprime un fichier
touch fichier.txt  # Crée un fichier
mkdir mon_dossier  # Crée un dossier

# Redirections
commande > fichier.txt  # Redirige la sortie standard
commande >> fichier.txt  # Ajoute à la fin du fichier
commande 2> erreur.txt  # Redirige la sortie erreur
commande &> sortie.txt  # Redirige sortie et erreur

# Pipes
ls -l | grep "txt"  # Filtre les fichiers txt

# Gestion des processus
ps aux  # Liste les processus
kill 1234  # Tue un processus
nohup commande &  # Exécute en arrière-plan

# Substitution de commande
DATE=$(date +%Y-%m-%d)
echo "Nous sommes le $DATE"

# Extraction de texte
cut -d":" -f1 /etc/passwd  # Coupe les champs
awk '{print $1}' fichier.txt  # Affiche la première colonne
sed 's/ancien/nouveau/g' fichier.txt  # Remplace du texte

# Permissions
chmod +x script.sh  # Rend exécutable
chown user:group fichier.txt  # Change le propriétaire

# Variables d'environnement
export VAR="test"
env | grep VAR  # Affiche les variables

# Commandes utiles
history  # Affiche l'historique
alias ll='ls -l'
unalias ll  # Supprime un alias
```