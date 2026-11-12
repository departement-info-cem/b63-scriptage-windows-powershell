---
titre: Quiz - Les tableaux (array)!
duree: 15
---


## Question 1 – On exécute les commandes suivantes :

```
$ordinateurs = @("PC01", "PC02", "PC03", "PC04")
$ordinateurs[2]
```

Quel résultat sera affiché ?


- [ ] PC01
- [ ] PC02
- [x] PC03
- [ ] PC04


## Question 2 – On souhaite connaître le nombre de fichiers retournés par la commande suivante :

```
$fichiers = Get-ChildItem C:\Windows
```

Quelle commande permet d'obtenir directement le nombre d'éléments du tableau ?


- [ ] `$fichiers.Count()`
- [x] `$fichiers.Length`
- [ ] `Get-Count $fichiers`
- [ ] `$fichiers.Size`


## Question 3 – On souhaite créer un tableau vide nommé `$utilisateurs`, puis y ajouter deux noms.

Quelle séquence de commandes est correcte ?

Proposition 1

```
$utilisateurs = $null
$utilisateurs = "Ali"
$utilisateurs = "Sara"
```

Proposition 2

```
$utilisateurs = @()
$utilisateurs += "Ali"
$utilisateurs += "Sara"
```

Proposition 3

```
$utilisateurs = ""
$utilisateurs += "Ali"
$utilisateurs += "Sara"
```

Proposition 4

```
$utilisateurs = []
$utilisateurs += "Ali"
$utilisateurs += "Sara"
```


- [ ] Proposition 1
- [x] Proposition 2
- [ ] Proposition 3
- [ ] Proposition 4


## Question 4 – On dispose du tableau suivant :

```
$notes = @(45, 72, 85, 50, 91, 68)
```

Quelle commande permet d'obtenir uniquement les notes supérieures ou égales à 70 ?


- [x] `$notes | Where-Object { $_ -ge 70 }`
- [ ] `$notes | Where-Object { $_ -le 70 }`
- [ ] `$notes | Select-Object { $_ -ge 70 }`
- [ ] `$notes | ForEach-Object { $_ -ge 70 }`


## Question 5 – On exécute les commandes suivantes :

```
$services = @("Spooler", "W32Time", "WinRM", "BITS")
$services[0,2]
```

Quel résultat sera obtenu ?


- [ ] Spooler et W32Time
- [ ] W32Time et WinRM
- [x] Spooler et WinRM
- [ ] WinRM et BITS


## Question 6 – On utilise le tableau suivant :

```
$roles = @("Administrateur", "Utilisateur", "Invité")
```

Quelle expression permet de vérifier si le tableau contient la valeur `Administrateur` ?


- [x] `$roles -Contains "Administrateur"`
- [ ] `$roles -Like "Administrateur"`
- [ ] `$roles -Match "Administrateur"`
- [ ] `$roles -Equal "Administrateur"`


## Question 7 – On exécute les commandes suivantes :

```
$groupeA = @("Ali", "Sara")
$groupeB = @("Mourad", "Nadia")

$groupeC = $groupeA + $groupeB
```

Quel sera le contenu de `$groupeC` ?


- [ ] Ali, Sara
- [ ] Mourad, Nadia
- [x] Ali, Sara, Mourad, Nadia
- [ ] Ali, Sara, Ali, Sara


## Question 8 – On exécute les commandes suivantes :

```
$groupeA = @("Ali", "Sara", "Mourad")
$groupeB = @("Sara", "Nadia")

$groupeC = $groupeA + $groupeB | Select-Object -Unique
```

Quel sera le contenu du tableau `$groupeC` ?


- [ ] Ali, Sara, Mourad, Sara, Nadia
- [x] Ali, Sara, Mourad, Nadia
- [ ] Sara uniquement
- [ ] Nadia uniquement


## Question 9 – On exécute les commandes suivantes :

```
$nombres = @(15, 3, 22, 8, 10)
$nombres | Sort-Object -Descending
```

Quel sera le résultat affiché ?


- [ ] 3, 8, 10, 15, 22
- [x] 22, 15, 10, 8, 3
- [ ] 15, 3, 22, 8, 10
- [ ] 22, 10, 15, 8, 3


## Question 10 – On exécute la commande suivante :

```
1..4 | ForEach-Object {
    Write-Host "Ordinateur $_"
}
```

Qu'est-ce qui sera affiché ?

Proposition 1

```
Ordinateur 1
Ordinateur 2
Ordinateur 3
Ordinateur 4
```

Proposition 2

```
Ordinateur $_
Ordinateur $_
Ordinateur $_
Ordinateur $_
```

Proposition 3

```
Ordinateur 4
```

Proposition 4

La commande génère une erreur.


- [x] Proposition 1
- [ ] Proposition 2
- [ ] Proposition 3
- [ ] Proposition 4



## Question 1 – Quelle instruction permet de créer un tableau contenant les valeurs 10, 20 et 30 ?

```
```


- [x] `$nombres = 10, 20, 30`
- [ ] `$nombres = "10;20;30"`
- [ ] `$nombres = 10 + 20 + 30`
- [ ] `$nombres = (10..30)`


## Question 2 – On exécute les instructions suivantes :

```
$nombres = 10, 20, 30, 40
$nombres[2]
```

Quelle valeur sera affichée ?


- [ ] 10
- [ ] 20
- [x] 30
- [ ] 40


## Question 3 – Dans PowerShell, quel est l'indice du premier élément d'un tableau ?


- [ ] 1
- [x] 0
- [ ] -1
- [ ] 2


## Question 4 – On exécute les instructions suivantes :

```
$notes = 12, 15, 18, 14, 16
$notes.Count
```

Quelle valeur sera affichée ?


- [ ] 4
- [x] 5
- [ ] 16
- [ ] 75


## Question 5 – On exécute les instructions suivantes :

```
$couleurs = "Rouge", "Vert", "Bleu"
$couleurs[1] = "Jaune"
$couleurs[1]
```

Quelle valeur sera affichée ?


- [ ] Rouge
- [x] Jaune
- [ ] Vert
- [ ] Bleu


## Question 6 – On exécute les instructions suivantes :

```
$nombres = 10, 20, 30
$nombres += 40
$nombres.Count
```

Quelle valeur sera affichée ?

- [ ] 3
- [x] 4
- [ ] 40
- [ ] 100


## Question 7 – On exécute le code suivant :

```
$nombres = 2, 4, 6

foreach ($nombre in $nombres) {
    Write-Output ($nombre * 2)
}
```

Quelles valeurs seront affichées, dans l'ordre ?

- [ ] 2, 4, 6
- [x] 4, 8, 12
- [ ] 4, 6, 8
- [ ] 2, 8, 12


## Question 8 – On exécute les instructions suivantes :

```
$jours = "Lundi", "Mardi", "Mercredi", "Jeudi"
$jours[-1]
```

Quelle valeur sera affichée ?


- [ ] Lundi
- [ ] Mardi
- [ ] Mercredi
- [x] Jeudi


## Question 9 – On exécute les instructions suivantes :

```
$nombres = 5, 10, 15
$total = 0

foreach ($nombre in $nombres) {
    $total += $nombre
}

$total
```

Quelle valeur sera affichée ?


- [ ] 15
- [ ] 20
- [x] 30
- [ ] 35


## Question 10 – On exécute les instructions suivantes :

```
$animaux = "Chat", "Chien", "Lapin"
$animaux[5]
```

Quel sera le résultat ?


- [ ] Chat
- [ ] Une erreur indiquant que l'indice est invalide.
- [x] La valeur `$null`.
- [ ] Le dernier élément du tableau.


