---
titre: Quiz - Types de données
duree: 15
---


## Question 1 – Identifier le type d'une variable**

On exécute les instructions suivantes :

```powershell
$age = 25
$prix = 19.99
$nom = "Ali"

$age.GetType().Name
$prix.GetType().Name
$nom.GetType().Name
```

Quels seront les trois types affichés, dans l'ordre ?

- [x] `Int32`, `Double`, `String`
- [ ] `Int64`, `Decimal`, `String`
- [ ] `Int32`, `Single`, `Char`
- [ ] `String`, `Double`, `String`



## Question 2 – Conversion de type**

On exécute :

```powershell
$valeur = "150"
$resultat = [int]$valeur

$resultat.GetType().Name
```

Quel sera le résultat ?

- [ ] `String`
- [x] `Int32`
- [ ] `Double`
- [ ] Une erreur de conversion

## Question 3 – Conversion incompatible**

On exécute :

```powershell
$valeur = "Bonjour"
$nombre = [int]$valeur
```

Que se passe-t-il ?

- [ ] La variable `$nombre` contient 0.
- [ ] La variable `$nombre` contient 1.
- [x] Une erreur de conversion est générée.
- [ ] La variable `$nombre` contient la chaîne "Bonjour".

## Question 4 – Dépassement de capacité**

On exécute :

```powershell
$nombre = [byte]256
```

Sachant que le type `[byte]` accepte les valeurs de 0 à 255, que se passe-t-il ?

- [ ] La variable contient 256.
- [ ] La variable contient 255.
- [x] Une erreur de conversion est générée.
- [ ] La variable contient 0.

## Question 5 – Type booléen**

On exécute :

```powershell
$actif = $false
$actif = -not $actif

$actif
```

Quelle valeur sera affichée ?

- [x] `True`
- [ ] `False`
- [ ] 0
- [ ] `$null`

## Question 6 – Tableau et type des éléments**

On exécute :

```powershell
$nombres = @(10, 20, 30)

$nombres.GetType().Name
$nombres[1]
```

Quels résultats seront affichés ?

- [ ] `Int32` puis `20`
- [x] `Object[]` puis `20`
- [ ] `Array` puis `10`
- [ ] `String[]` puis `30`

## Question 7 – Conversion et calcul**

On exécute :

```powershell
$a = "10"
$b = "5"

$resultat = [int]$a + [int]$b

$resultat
```

Quelle valeur sera affichée ?

- [ ] `105`
- [x] `15`
- [ ] `10 + 5`
- [ ] Une erreur de conversion

## Question 8 – Utilisation des suffixes de mémoire**

On exécute :

```powershell
$taille = 2Mb
$taille
```

Quelle valeur sera affichée ?

- [ ] 2 000 000
- [ ] 2 048
- [x] 2 097 152
- [ ] 1 048 576

## Question 9 – Valeurs minimum et maximum**

On exécute :

```powershell
[int]::MaxValue
```

Quelle valeur sera retournée ?

- [ ] 32 767
- [ ] 65 535
- [x] 2 147 483 647
- [ ] 4 294 967 295

## Question 10 – Différence entre Hashtable et PSCustomObject**

On exécute :

```powershell
$utilisateur = [pscustomobject]@{
    Nom = "Ali"
    Age = 25
}

$utilisateur.Nom
$utilisateur.Age
```

Quels résultats seront affichés ?

- [x] `Ali` puis `25`
- [ ] `Nom` puis `Age`
- [ ] Une erreur, car les propriétés ne sont pas accessibles avec le point.
- [ ] Un seul objet contenant le texte `Nom = Ali, Age = 25`.