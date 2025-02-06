```
# Exécution d'un script
./script.ps1  # Exécute un script (nécessite Set-ExecutionPolicy)
powershell -File script.ps1  # Exécute avec PowerShell directement

# Variables
$VAR = "valeur"
Write-Output $VAR  # Affiche la valeur

# Lecture d'entrée
$VAR = Read-Host "Entrer une valeur"

# Conditions
if ($VAR -eq "test") {
    Write-Output "Match!"
}

# Boucles
for ($i=1; $i -le 5; $i++) { Write-Output "Itération $i" }

while ($VAR -ne "quit") {
    $VAR = Read-Host "Entrer une valeur"
    Write-Output "Vous avez tapé : $VAR"
}

# Fonctions
function Ma-Fonction {
    param([string]$nom)
    Write-Output "Hello, $nom"
}
Ma-Fonction "World"

# Opérations sur fichiers
Get-ChildItem fichier.txt  # Liste détaillée
Remove-Item fichier.txt  # Supprime un fichier
New-Item fichier.txt -ItemType File  # Crée un fichier
New-Item mon_dossier -ItemType Directory  # Crée un dossier

# Redirections
Commande > fichier.txt  # Redirige la sortie standard
Commande >> fichier.txt  # Ajoute à la fin du fichier
Commande 2> erreur.txt  # Redirige la sortie erreur
Commande *> sortie.txt  # Redirige sortie et erreur

# Pipes
Get-ChildItem | Where-Object { $_.Name -like "*.txt" }  # Filtre les fichiers txt

# Gestion des processus
Get-Process  # Liste les processus
Stop-Process -Id 1234  # Tue un processus
Start-Process -NoNewWindow commande  # Exécute en arrière-plan

# Substitution de commande
$DATE = Get-Date -Format "yyyy-MM-dd"
Write-Output "Nous sommes le $DATE"

# Extraction de texte
Get-Content fichier.txt | ForEach-Object { $_ -split ":" } | Select-Object -First 1  # Coupe les champs
Get-Content fichier.txt | ForEach-Object { ($_ -split " ")[0] }  # Affiche la première colonne
(Get-Content fichier.txt) -replace "ancien", "nouveau" | Set-Content fichier.txt  # Remplace du texte

# Permissions
Set-ExecutionPolicy RemoteSigned  # Permet d'exécuter des scripts
icacls fichier.txt /grant User:F  # Change les permissions

# Variables d'environnement
$env:VAR = "test"
Get-ChildItem Env:VAR  # Affiche la variable

# Commandes utiles
Get-History  # Affiche l'historique
Set-Alias ll Get-ChildItem
Remove-Item Alias:ll  # Supprime un alias
```