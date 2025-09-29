---
title: Paramètres et dictionnaires
sidebar_label: Paramètres et dictionnaires
toc_max_heading_level: 3
---


## 1. Configuration du débogueur dans VS Code

Avant de commencer, vous devez apprendre à configurer votre débogueur pour injecter des paramètres.

Ouvrez un **espace de travail** (un répertoire qui contiendra vos sources). Vous pouvez utiliser `C:\EspaceLabo`, qui existe déjà. Pour ouvrir ce répertoire dans votre espace de travail, vous pouvez soit faire `Maj + Clic droit` sur le répertoire dans l'explorateur et sélectionner **"Ouvrir avec Code"**, ou encore **Fichier / Ouvrir le dossier** dans VS Code.

Dans le panneau de gauche, dans la section Explorateur, créez un nouveau fichier `addition.ps1` et mettez-y le code suivant:

```powershell showLineNumbers
    param(
        [double[]] $termes
    )

    $total = 0

    foreach ($terme in $termes) {
        $total += $terme
    }

    $total
```

Insérez **un point d'arrêt** à la première ligne exécutée (`$total = 0`). Appuyez sur **F5**. Notez que le débogueur ne vous est pas d'une grande utilité, car aucun paramètre n'a pu être passé.

Dans le panneau "Exécuter et déboguer", à gauche, cliquez sur **"Créer un fichier launch.json"**. Sélectionnez **"Launch current file"**. Cela créera un fichier `launch.json` dans un sous-répertoire `.vscode` dans votre espace de travail.

Modifiez le fichier launch.json pour spécifier les arguments à utiliser. Par exemple, si vous voulez déboguer le script avec la ligne de commande suivante:

```
PS C:\EspaceLabo> .\addition.ps1 -Termes 2, 3, 5
```

Vous devriez mettre `"-Termes 2, 3, 5"` entre les crochets dans l'attribut `args`. Comme ceci:

```json
{
    // Utilisez IntelliSense pour en savoir plus sur les attributs possibles.
    // Pointez pour afficher la description des attributs existants.
    // Pour plus d'informations, visitez : https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "PowerShell: Launch Current File",
            "type": "PowerShell",
            "request": "launch",
            "script": "${file}",
            "args": ["-Termes 2, 3, 5"]
        }
    ]
}
```

Enregistrez le fichier, puis retournez dans votre script et appuyez sur F5 pour démarrer le script en mode débogage.

Vous savez maintenant comment déboguer des scripts avec arguments!
:::

---

## 2. Paramètres

Dans cet exercice, vous aller créer des scripts qui prennent en charge des paramètres. Pour utiliser le débogueur avec des arguments, suivez **la procédure à la fin de cette page**.

:::tip
Pour tester votre script avec le débogueur ou avec la touche **F5**, assurez-vous de modifier les arguments dans le fichier **launch.json**.
:::


**A)** Écrivez un script `Multiplier.ps1` qui écrit dans le pipeline le résultat de la multiplication de deux termes passés en paramètre. (Par exemple, ".\Multiplier.ps1 -Terme1 3 -Terme2 5" écrirait le résultat de 3x5)

```
PS C:\Scripts> .\multiplier.ps1 -Terme1 3 -Terme2 5
15
```


**B)** Réécrivez le script précédent, mais de sorte que les paramètres soient obligatoires.

```
PS C:\Scripts> .\multiplier.ps1 -Terme1 3

cmdlet multiplier.ps1 at command pipeline position 1
Supply values for the following parameters:
Terme2: 5
15
```


**C)** Réécrivez le script précédent, mais de sorte qu'il puisse admettre un nombre illimité de nombres. 

*(Rappelez-vous que l'élément neutre de la multiplication est 1)*

```
PS C:\Scripts> .\multiplier.ps1 -Termes 3, 5, 2
30
```

---

## 3. Dictionnaires

**A)** Dans une variable, créez la structure suivante:

```
PS C:\Scripts> $Luke | Format-List

Nom        : Skywalker
Prenom     : Luke
Profession : Jedi
```

**B)** Faites une variable qui contient ces trois personnages:

```
PS C:\Scripts> $Personnages

Nom       Prenom  Profession
---       ------  ----------
Skywalker Luke    Jedi
Kenobi    Obi Wan Jedi
Yoda      Minch   Jedi
```

**C)** Ajoutez Darth Vader à ce tableau.

```
PS C:\Scripts> $Personnages

Nom       Prenom  Profession
---       ------  ----------
Skywalker Luke    Jedi
Kenobi    Obi Wan Jedi
Yoda      Minch   Jedi
Vader     Darth   Sith
```


**D)** Dressez la liste de tous les jedis seulement.

```
Nom       Prenom  Profession
---       ------  ----------
Skywalker Luke    Jedi
Kenobi    Obi Wan Jedi
Yoda      Minch   Jedi
```


**E)** Faites un **script** qui produit un personnage et l'envoie dans le pipeline.

```
PS C:\Scripts> .\monscript.ps1 -Nom "Solo" -Prenom "Han" -Profession "Contrebandier" | Format-List

Nom        : Solo
Prenom     : Han
Profession : Contrebandier


PS C:\Scripts> .\monscript.ps1 -Nom "Organa" -Prenom "Leia" -Profession "Princesse" | Format-Table

Nom    Prenom Profession
---    ------ ----------
Organa Leia   Princesse
```


**F)** Appelez ce script en utilisant la technique du ***Splatting***.


---

## 4. Validation des paramètres


Le script suivant est utilisé par les policiers lorsqu'ils interpellent un(e) automobiliste pour excès de vitesse. Il produit en sortie un objet qui contiennent l'information de l'incident,

```powershell showLineNumbers
Param(
    [string] $Nom,              # Nom de l'automobiliste
    [string] $Prenom,           # Prénom de l'automobiliste
    [string] $NoPermis,         # Numéro de permis (X9999-999999-99)
    [string] $TypeVehicule,     # Le type de véhicule (Voiture, Camion, Moto, Autobus, Autre)
    [string] $Immatriculation,  # Le numéro de plaque d'immatriculation
    [switch] $Confiscation      # Spécifie si le véhicule doit être confisqué
    [string] $Infraction        # Le détail de l'infraction
    [int]    $Points,           # Le nombre de points d'inaptitude à ajouter au dossier
)


[PSCustomObject]@{
    Nom = $Nom
    Prenom = $Prenom
    NoPermis = $NoPermis
    TypeVehicule = $TypeVehicule
    Immatriculation = $Immatriculation
    Confiscation = $Confiscation
    Infraction = $Infraction
    Points = $Points
}

```


Modifiez ce script pour qu'il effectue les validations suivantes:

**A)** Le nom et le prénom doivent être obligatoires

**B)** Le nombre de points d'inaptitude doit être un nombre entier compris entre 0 et 30

**C)** Le type du véhicule doit être l'un des 5 suivants: Voiture, Camion, Moto, Autobus, Autre

**D)** Le numéro de plaque, s'il est spécifié, doit être une chaîne de 2 à 7 caractères.


:::info Question optionnelle
**E)** 🏆 Le numéro de permis de conduire, s'il est spécifié, doit correspondre à la nomenclature ci-dessous. Utilisez une validation basée sur un *pattern regex*.

    > `1 lettre` , `4 chiffres` , `trait d'union` , `6 chiffres` , `trait d'union` , `2 chiffres`

    Vous n'avez pas vu les *regex* en classe et ce ne sera pas à l'examen, mais si ça vous intéresse et que vous avez terminé les autres exercices, vous pouvez regarder [cette section des notes de cours](/notions/powershell/strings#expressions-régulières-regex).

:::

