---
titre: Quiz - Opérateurs
duree: 15
---

## Question 1 – Opérateur modulo**

On exécute les instructions suivantes :

```powershell
$a = 17
$resultat = $a % 5
$resultat
```

Quelle valeur sera affichée ?

- [x] 2
- [ ] 3
- [ ] 3.4
- [ ] 5


## Question 2 – Division et conversion de type**

On exécute :

```powershell
$resultat = [int](17 / 5)
$resultat
```

Quelle valeur sera affichée ?

- [x] 3
- [ ] 3.4
- [ ] 4
- [ ] 5


## Question 3 – Opérateur de comparaison**

On exécute :

```powershell
$age = 18
$age -ge 18
```

Quelle valeur sera retournée ?

- [x] `True`
- [ ] `False`
- [ ] 18
- [ ] `$null`


## Question 4 – Comparaison avec un wildcard**

On exécute :

```powershell
$nom = "serveur01"
$nom -like "serveur*"
```

Quel sera le résultat ?

- [x] `True`
- [ ] `False`
- [ ] `serveur`
- [ ] Une erreur de syntaxe


## Question 5 – Opérateur logique AND**

On exécute :

```powershell
$age = 25
$permis = $true

($age -ge 18) -and $permis
```

Quelle valeur sera retournée ?

- [x] `True`
- [ ] `False`
- [ ] 25
- [ ] `$null`

## Question 6 – Opérateur logique OR**

On exécute :

```powershell
$note = 45

($note -ge 60) -or ($note -lt 50)
```

Quelle valeur sera retournée ?

- [x] `True`
- [ ] `False`
- [ ] 45
- [ ] Une erreur

## Question 7 – Affectation avec addition**

On exécute :

```powershell
$compteur = 10
$compteur += 5
$compteur += 3

$compteur
```

Quelle valeur sera affichée ?

- [ ] 13
- [ ] 15
- [x] 18
- [ ] 23


## Question 8 – Affectation avec modulo**

On exécute :

```powershell
$nombre = 23
$nombre %= 4

$nombre
```

Quelle valeur sera affichée ?

- [x] 3
- [ ] 4
- [ ] 5.75
- [ ] 7


## Question 9 – Incrémentation et décrémentation**

On exécute :

```powershell
$i = 5
$i++
$i++
$i--
$i
```

Quelle valeur sera affichée ?

- [ ] 5
- [x] 6
- [ ] 7
- [ ] 8


## Question 10 – Opérateur de comparaison de type**

On exécute :

```powershell
$valeur = 12.5
$valeur -is [double]
```

Quelle valeur sera retournée ?

- [x] `True`
- [ ] `False`
- [ ] 12.5
- [ ] Une erreur de conversion


