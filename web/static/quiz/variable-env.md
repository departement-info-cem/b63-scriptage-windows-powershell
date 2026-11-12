---
titre: Quiz - Variables d'environnement!
duree: 15
---


## Question 1 – Quelle syntaxe permet d'afficher le nom de l'utilisateur actuellement connecté à Windows ?


- [ ] `$USER`
- [x] `$env:USERNAME`
- [ ] `$USERNAME`
- [ ] `env:USERNAME`


## Question 2 – Que retourne la commande suivante ?

```
$env:WINDIR
```


- [ ] Le répertoire personnel de l'utilisateur.
- [x] Le répertoire d'installation de Windows.
- [ ] Le répertoire temporaire.
- [ ] Le répertoire contenant les programmes installés.


## Question 3 – Quelle commande permet d'afficher le contenu de la variable d'environnement `COMPUTERNAME` ?


- [ ] `Get-Variable COMPUTERNAME`
- [x] `Write-Output $env:COMPUTERNAME`
- [ ] `Get-Environment COMPUTERNAME`
- [ ] `Show-Variable COMPUTERNAME`


## Question 4 – Que représente la variable d'environnement suivante ?

```
$env:USERPROFILE
```

- [ ] Le répertoire d'installation de Windows.
- [x] Le répertoire personnel de l'utilisateur connecté.
- [ ] Le répertoire contenant les processus Windows.
- [ ] Le répertoire temporaire de Windows.

## Question 5 – Que fait l'instruction suivante ?

```
$env:APP_MODE = "Production"
```


- [ ] Elle crée une variable PowerShell ordinaire appelée `APP_MODE`.
- [x] Elle définit la variable d'environnement `APP_MODE` à `Production`.
- [ ] Elle modifie une variable d'environnement permanente pour tous les utilisateurs.
- [ ] Elle supprime la variable d'environnement `APP_MODE`.


## Question 6 – On exécute la commande suivante dans PowerShell :

```
$env:APP_MODE = "Test"
```

Quelle affirmation est correcte ?

- [ ] La modification est automatiquement enregistrée dans le Registre Windows.
- [x] La modification s'applique à la session PowerShell actuelle et aux processus enfants lancés ensuite.
- [ ] La modification est automatiquement appliquée à tous les utilisateurs Windows.
- [ ] La modification est conservée après le redémarrage de Windows.


## Question 7 – Quelle commande permet de supprimer la variable d'environnement `APP_MODE` de la session actuelle ?


- [ ] `Remove-Variable APP_MODE`
- [x] `Remove-Item Env:APP_MODE`
- [ ] `Clear-Variable Env:APP_MODE`
- [ ] `Delete-Environment APP_MODE`


## Question 8 – Quelle commande permet d'afficher toutes les variables d'environnement accessibles dans la session PowerShell ?


- [ ] `Get-Variable`
- [x] `Get-ChildItem Env:`
- [ ] `Get-Process Env:`
- [ ] `Get-Environment`


## Question 9 – Quel est le rôle de la variable d'environnement `PATH` ?


- [ ] Elle indique le nom de l'utilisateur connecté.
- [ ] Elle indique le répertoire d'installation de Windows.
- [x] Elle contient une liste de répertoires dans lesquels Windows peut rechercher des programmes exécutables.
- [ ] Elle contient la liste des processus en cours d'exécution.


## Question 10 – On souhaite ajouter le répertoire `C:\Outils` à la variable `PATH` sans supprimer les chemins déjà présents.

Quelle instruction faut-il utiliser ?


- [ ] `$env:PATH = "C:\Outils"`
- [x] `$env:PATH += ";C:\Outils"`
- [ ] `$env:PATH - "C:\Outils"`
- [ ] `Remove-Item Env:PATH`


## Question 11 – Que retourne la commande suivante ?

```
$env:TEMP
```


- [ ] Le répertoire contenant les fichiers système.
- [ ] Le répertoire personnel de l'utilisateur.
- [x] Le chemin du répertoire temporaire de l'utilisateur.
- [ ] Le répertoire contenant les programmes installés.


## Question 12 – On exécute les instructions suivantes :

```
$env:APP_MODE = "Test"

Test-Path Env:APP_MODE
```

Quel sera le résultat ?


- [ ] `$null`
- [ ] `1`
- [x] `True`
- [ ] `False`


## Question 13 – Quelle est la différence entre les deux instructions suivantes ?

```
$nom = "Jamil"
$env:NOM = "Jamil"
```


- [ ] Les deux instructions créent exactement le même type de variable.
- [x] La première crée une variable PowerShell et la deuxième crée une variable d'environnement.
- [ ] La première crée une variable d'environnement et la deuxième une variable PowerShell.
- [ ] La deuxième instruction crée automatiquement un compte Windows.


## Question 14 – Que représente la variable d'environnement suivante ?

```
$env:USERPROFILE
```


- [x] Le chemin du répertoire personnel de l'utilisateur connecté.
- [ ] Le chemin du répertoire courant de PowerShell.
- [ ] Le chemin du répertoire d'installation de PowerShell.
- [ ] Le chemin du répertoire contenant les services Windows.


## Question 15 – On exécute les instructions suivantes :

```
Remove-Item Env:APP_MODE -ErrorAction SilentlyContinue

$env:APP_MODE
```

Quel sera le résultat de la deuxième instruction ?


- [ ] Une chaîne vide.
- [ ] Une erreur indiquant que la variable n'existe pas.
- [x] La valeur `$null`.
- [ ] La valeur `False`.


