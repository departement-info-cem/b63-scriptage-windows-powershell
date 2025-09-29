---
title: Fonctions et journalisation
sidebar_label: Fonctions et journalisation
toc_max_heading_level: 2
---


## 1. Fonctions

**A)** Dans le fichier `Invoke-Addition.PS1`, définissez **une fonction** `Invoke-Addition` qui prend en paramètre un nombre illimité de termes.

```powershell showLineNumbers
function Invoke-Addition {
    param(
        [double[]] $termes
    )

    $total = 0

    foreach ($terme in $termes) {
        $total += $terme
    }

    $total
}
```

Testez ce script avec `F5`. Celui-ci ne donne aucun résultat. **Pourquoi selon vous?**


Pour tester votre fonction, appelez-la dans le code principal de votre script, comme ceci:

```powershell showLineNumbers
function Invoke-Addition {
    # le code de la fonction...
}

Invoke-Addition 2, 3, 5
```

Cependant, même si la fonction prend des paramètres, le script n'en prend pas. Ce n'est pas pratique!




**B)** Tentez de lancer votre script à l'aide de la méthode du **dot-sourcing**. Puis, dans le même terminal, tentez de lancer la fonction.

```
PS C:\EspaceLabo> . .\Invoke-Addition.ps1

PS C:\EspaceLabo> Invoke-Addition 2, 4, 6
```

---

## 2. Pile d'appel (trace)

On veut accomplir trois éléments principaux:
1. Indiquer quelles lignes de code s'exécutent dans quel ordre
2. Indiquer les effets de chaque ligne de code (valeur des variables à l'intérieur de la portée)
3. Indiquer la pile d'appel, la courante en haut

### Exemple

Si on a le code suivant :

```powershell  showLineNumbers
function MaFonction ([int]$x, [int]$y) {
    $z = $x + $y
    return $z
}

$a = 3
$b = 5
$c = MaFonction $a $b
Write-Host "La réponse est $c"
```

On aura la trace suivante :

| Ligne exécutée                 | Effet                                                  | Pile d'appel              |
|:-------------------------------|:-------------------------------------------------------|:--------------------------|
| $a = 3                         | $a: 3                                                  | **Script**                |
| $b = 5                         | $a: 3 ; $b: 5                                          | **Script**                |
| $c = MaFonction $a $b          | $a: 3 ; $b: 5<br/>Appelle MaFonction avec $x=3 et $y=5 | **Script**                |
| $z = $x + $y                   | $x: 3 ; $y: 5 ; $z: 8                                  | **MaFonction**<br/>Script |
| return $z                      | $x: 3 ; $y: 5 ; $z: 8<br/>Sortie: 8                    | **MaFonction**<br/>Script |
| $c = MaFonction $a $b          | $a: 3 ; $b: 5 ; $c: 8                                  | **Script**                |
| Write-Host "La réponse est $c" | Écrit "La réponse est 8"                               | **Script**                |


### Deuxième trace

Sur un médium de votre choix, suivez l'exécution de ce script dans votre tête et remplissez un tableau de trace pour chaque ligne, dans l'ordre de leur exécution.

```powershell showLineNumbers
function X ([int]$x) {
    $xx = $x + 80
    return $xx
}
function Y ([int]$y) {
    $yy = X -x ($y * 3)
    return $yy
}

$a = X -x 10
$b = Y -y 17
$c = $b - $a + 1
Write-Host "La réponse est $c"
```

Une fois terminé, copiez ce code dans un fichier PS1, insérez un point d'arrêt **à la première ligne exécutée** et validez votre réponse à l'aide du débogueur.


---

## 3. Journalisation

:::info
En faisant cet exercice, vous prenez de l'avance sur votre TP2, puisque vous pourrez réutiliser cette fonction!
:::

**a)** Écrivez une fonction `Write-Log` qui prend en paramètre **un message obligatoire non nul** pouvant également entrer par le **pipeline**. Cette fonction doit écrire le message dans un fichier **MonJournal.log** sauvegardé dans un sous-répertoire MonJournal contenu **dans le répertoire temporaire de l'utilisateur** (la fonction doit le créer s'il n'existe pas). Le message doit être préfixé de la date et l'heure dans le format montré dans l'exemple (c'est un timestamp).

Faites un script dans lequel votre fonction est définie au début. Appelez la fonction à quelques endroits dans le script après la fonction pour tester.

```powershell showLineNumbers
function Write-Log {
    # Écrivez votre belle fonction ici...
}

Write-Log -Message "Un premier message"

Write-Log -Message "Un deuxième message"

"Un message passé par le pipeline" | Write-Log
```

Le fichier devrait ressembler à ceci (notez le format du *timestamp*).

```
2025-09-30 08:11:46.180       Un premier message
2025-09-30 08:11:46.185       Un deuxième message
2025-09-30 08:11:46.208       Un message passé par le pipeline
```

Le fichier devrait se trouver dans le profil utilisateur, dans le répertoire suivant:

```
C:\Users\NomDeLutilisateur\AppData\Local\Temp\MonJournal\
```

:::tip Quelques conseils
- Le répertoire temporaire de l'utilisateur est défini dans la variable d'environnement `$env:TEMP`.
- Utilisez la commande `Get-Date` pour générer le timestamp. Spécifiez le format avec `-Format` ([voir la documentation](https://learn.microsoft.com/fr-ca/dotnet/standard/base-types/custom-date-and-time-format-strings)).
:::

