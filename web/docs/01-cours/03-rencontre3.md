---
title: R03 - Dictionnaires et scripts
slug: "03"
draft: false
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Rencontre 3 - Dictionnaires et scripts

:::note Résumé de la séance du Lundi 8 juillet 2024

<Tabs>

<TabItem value="deroulement" label="👨‍🏫 Déroulement du cours">

1. Rappel du cours précédent
1. Scripts
    1. Stratégies d'exécution et mesures de sécurité
    1. Encodage (UTF-8 avec ou sans BOM)
    1. Visual Studio Code
    1. Structures de contrôle
1. Structures de contrôle
    1. Conditions (if, switch)
    1. Boucles (for, while, do, foreach, Foreach-Object)
1. Structures de données en dictionnaire
    1. Objets [Hashtable]
    1. Objets [PSCustomObject]
    1. Splatting
1. Bonnes pratiques

</TabItem>

<TabItem value="exercices" label="💻 Exercices à compléter">

- Mini-exercice #3
- Exercices 03a et 03b
- [TP#1](/tp/tp1) à remettre pour le 18 juillet 2024 à 23h59

</TabItem>

<TabItem value="ressources" label="📚 Ressources à consulter">

La présentation PowerPoint est sur le Teams du cours, sous le canal Général > Fichiers > Supports de cours.

</TabItem>

</Tabs>

:::

## Scripts

Comme la plupart des langages de script, PowerShell est un langage interprété. Un fichier de script est donc un fichier texte standard contenant des lignes de commandes PowerShell. Lorsqu'on le passe dans l'interpréteur PowerShell, celui-ci exécute son contenu comme si chaque ligne du fichier était saisie une après l'autre dans la console. L'interpréteur reconnait le format d'un script PowerShell grâce à l'extension de fichier `.PS1`.

Plusieurs conditions doivent être respectées afin d'exécuter un script PowerShell avec succès, que ce soit pour des raisons de sécurité ou de compatibilité.

### Stratégie d'exécution (*ExecutionPolicy*)

Par défaut, sous Windows 10/11, l'exécution de scripts PowerShell est interdite pour des raisons de sécurité. Seuls les scripts qui possèdent une signature numérique *AuthentiCode* sont autorisés. Les systèmes d'exploitation Windows Server, quant à eux, ne bloquent l'exécution de scripts que lorsqu'ils proviennent d'Internet. Dans tous les cas, il est possible de modifier ce comportement. Ces règles sont définies dans la **stratégie d'exécution** (*execution policy*).

Il existe plusieurs niveaux de stratégies d'exécution, et chacun détermine dans quelles condition le système d'exploitation peut refuser ou permettre l'exécution d'un script PowerShell. 

| Stratégie d'exécution | Caractéristiques |
| -- | -- |
| `Restricted` | Lorsque la stratégie d'exécution est Restricted, seuls les scripts portant une signature numérique (authenticode) sont approuvés et peuvent être exécutés par l'interpréteur PowerShell. C'est la stratégie par défaut sur les clients Windows. |
| `RemoteSigned` | Les scripts provenant d'un médium de stockage local, de l'intranet ou d'un site Internet de confiance sont autorisés. Ceux provenant d'Internet seront bloqués à moins de porter une signature numérique ou d'être débloqués avec la commande Unblock-File. C'est la stratégie par défaut sur les serveurs Windows, et la stratégie recommandée sur les clients Windows faisant partie d'un domaine ou utilisés pour développer des scripts. |
| `AllSigned` | Cette stratégie oblige tous les scripts à posséder une signature numérique. |
| `Unrestricted` | Les scripts non signés provenant d'Internet peuvent être exécutés, mais en demandant la permission à l'utilisateur. |
| `Bypass` | Cette stratégie est la plus permissive: tous les scripts peuvent s'exécuter sans restriction. |

Pour connaître la stratégie en vigueur, on peut lancer la commande `Get-ExecutionPolicy`.

![image](./assets/r03/r05_01a.png)

Pour définir la stratégie, c'est la commande `Set-ExecutionPolicy` qu'il faut utiliser. 

![image](./assets/r03/r05_01b.png)

Normalement, il faut être administrateur de sa machine pour pouvoir définir la stratégie d'exécution (la commande doit être lancée avec des privilèges **élevés**). Mais il est possible de la définir dans le contexte de l'utilisateur local avec l'argument `-Scope CurrentUser`.

![image](./assets/r03/r05_01c.png)

Lorsque plusieurs stratégies sont définies, Windows utilise un ordre de priorité (appelé precedence) pour déterminer la valeur résultante. On peut voir la liste des stratégies d'exécution selon leur niveau de priorité en lançant la commande Get-ExecutionPolicy -List; la valeur effective est celle qui est retournée par Get-ExecutionPolicy sans spécifier d'argument.

![image](./assets/r03/r05_01d.png)

Les niveaux de priorité (ou Scope) sont les suivants:

| Scope | Description |
| -- | -- |
| `MachinePolicy` | Configuration de la stratégie au moyen de GPO machine, dans Active Directory. Les GPO machine ont une priorité absolue. |
| `UserPolicy` | Configuration de la stratégie au moyen de GPO utilisateur, dans Active Directory. |
| `Process` | Configuration de la stratégie effective pour le processus seulement. On définit cette stratégie lors de l'appel du programme powershell.exe |
| `CurrentUser` | Configuration de la stratégie par défaut pour l'utilisateur courant. Cette stratégie peut être modifiée sans disposer de droits d'administration. |
| `LocalMachine` | Configuration de la stratégie par défaut pour l'ordinateur local. Cette stratégie peut être modifiée seulement par un administrateur de la machine. C'est le niveau le moins prioritaire. |

Lorsque l'un ou l'autre des scopes est non défini (undefined), cela signifie que ce niveau est ignoré. La stratégie effective sera la plus haute dans la liste de priorité parmi toutes celles qui ne sont pas "undefined".

:::tip

Lorsqu'on démarre une session PowerShell à l'aide du programme powershell.exe, on peut spécifier une ExecutionPolicy différente de la configuration du système, qui ne vaut que pour cette instance précise de PowerShell. Pour ce faire, on doit indiquer la stratégie voulue à l'aide du paramètre -ExecutionPolicy. Cela aura pour effet de définir le scope Process, dont la priorité dépasse celle des préférences machine et utilisateur. Seules les GPO ont une priorité supérieure.

![image](./assets/r03/r05_01e.png)

:::


### Fichiers bloqués

Lorsqu'on télécharge un fichier sur Internet, Windows appose automatiquement une marque sur le fichier. Cette marque, appelée Zone Identifier, est conservée dans un champ alternatif (Alternate Data Stream), une structure dans le système de fichiers NTFS permettant de stocker des métadonnées. Si vous téléchargez un fichier script sur Internet et que vous le savez sécuritaire (par exemple, votre script que vous téléchargez sur OneDrive), vous pouvez le débloquer par l'interface graphique dans ses propriétés, ou encore à l'aide de la commande `Unblock-File`.

Un fichier ainsi bloqué sera impossible à exécuter si la stratégie d'exécution est AllSigned ou RemoteSigned, à moins d'avoir été dûment signé au moyen d'un certificat Authenticode en règle.

![image](./assets/r03/r05_02a.png)

![image](./assets/r03/r05_02b.png)


### Encodage

PowerShell 5.1 est assez capricieux sur l'encodage des caractères, surtout lorsqu'il est question de caractères accentués. Assurez-vous de sauvegarder les fichiers en format UTF8 avec BOM.

![image](./assets/r03/r05_03a.png)

Si l'encodage n'est pas adéquat, vous pourrez vous retrouver avec des caractères bizarres au lieu des lettres accentuées.

![image](./assets/r03/r05_03b.png)


### Exécution d'un script

Il y a plusieurs manières d'exécuter un script. Voici les principales:

#### Exécution par l'interface graphique

Vous pouvez lancer un script directement à partir de l'explorateur de fichiers sous Windows. Vous pourriez être tenté de double-cliquer sur le fichier de script, mais vous ne feriez qu'ouvrir le fichier dans le bloc-notes. Il faut plutôt cliquer avec le bouton de droite dans l'explorateur, puis sélectionner Exécuter avec PowerShell.

Cette méthode est certes intuitives, mais présente quelques inconvénients:
- Vous ne pouvez pas démarrer le script en tant qu'administrateur.
- La fenêtre se ferme toute seule une fois le script terminé, ce qui ne vous laisse pas le temps de voir s'il y a eu une erreur lors de l'exécution.
- Il est impossible de spécifier des arguments à la ligne de commande.

![image](./assets/r03/r05_04a.png)


#### Exécution par la console PowerShell

C'est la méthode la plus utile, car elle permet de spécifier des arguments et de voir le résultat du script à la console.

Il suffit de démarrer une console PowerShell (en lançant le programme powershell.exe, par le menu démarrer, par le menu Win+X, etc.) puis exécuter un fichier .ps1. Si le chemin du fichier est relatif, vous devez démarrer le chemin par `.\` (comme sous Linux).

![image](./assets/r03/r05_04b.png)

Si le script ne peut s'exécuter, lisez le message d'erreur. S'il est mention de la stratégie d'exécution (*ExecutionPolicy*), assurez-vous que votre stratégie soit définie à RemoteSigned ou plus permissif, et que votre fichier n'est pas bloqué (`Unblock-File`).


#### Exécution par l'invite de commande (cmd)

On peut lancer un script PowerShell à partir d'une commande classique, en lançant le programme `powershell.exe` avec l'argument `-file`.

![image](./assets/r03/r05_04c.png)

Il est aussi possible de contourner la stratégie d'exécution, en spécifiant l'argument `-ExecutionPolicy`.

![image](./assets/r03/r05_04d.png)


### Variables *$PSScriptRoot* et *$PSCommandPath*

Lorsque du code PowerShell est exécuté à travers un script, il est possible de récupérer de l'information sur ce fichier.

Vous pouvez connaître le chemin complet du script en interrogeant la variable `$PSCommandPath`.

![image](./assets/r03/script_pscommandpath.png)

Vous pouvez aussi connaître le chemin complet du répertoire dans lequel votre fichier de script est situé, en interrogeant la variable `$PSScriptRoot`

![image](./assets/r03/script_psscriptroot.png)

Notez que ces variables n'existent que pendant l'exécution d'un script. Elles sont vides sinon.

![image](./assets/r03/script_varconsole.png)


### Le caractère *BackTick*

Voici quelques caractères spéciaux qui doivent être spécifiés à l'aide du caractère d'échappement, le *backtick*, qui ressemble à un accent grave sans la lettre qui l'accompagne.

| Caractère | Hex. | Déc. | Description |
| -- | -- | -- | -- |
| \`r | 0x0D | 13 | Retour de chariot (carriage-return) |
| \`n | 0x0A | 10 | Saut de ligne (line-feed) - changement de ligne sur les systèmes UNIX
| \`r\`n | 0x0D, 0x0A | 13, 10 | CRLF (retour de chariot + saut de ligne) - changement de ligne sur Windows |
| \`t | 0x09 | 9 | Caractère de tabulation |
| \`f | 0x0C | 12 | Caractère de saut de page (pour les imprimantes) |



### Configuration

Je vous recommande d'activer le thème de couleur "PowerShell ISE", car il est optimisé pour PowerShell. Mais s'il ne vous plait pas, vous pouvez en essayer d'autres, et même en installer de nouveaux.

![image](./assets/r03/r05_06k.png)

Ajustez les paramètres de VS Code. Vous pouvez explorer les paramètres disponibles, mais voici minimalement ceux que je vous recommande:

Copiez-collez ceci dans le fichier settings.json, et sauvegardez-le.

```json
{
    "powershell.codeFormatting.preset": "Stroustrup",
    "powershell.codeFormatting.trimWhitespaceAroundPipe": true,
    "powershell.codeFormatting.useCorrectCasing": true,
    "powershell.integratedConsole.focusConsoleOnExecute": false,

    "[powershell]": {
        "files.encoding": "utf8bom",
        "editor.suggestSelection": "first",
        "editor.tabCompletion": "on",
        "editor.codeLens": false
    },

    "workbench.colorTheme": "PowerShell ISE"   
}
```


## Les structures de contrôle

A priori, les instructions contenues dans un fichier de script dont exécutées dans un ordre séquentiel, que l'on appelle le **flot de contrôle** (*control flow*). Les structures de contrôle sont des instructions qui permettent de dévier le flot de contrôle et ainsi rendre le programme dynamique.

Il existe quatre grands types de structures de contrôle:
- La **sélection**, qui permet d'ignorer des instructions sous certaines conditions;
- La **répétition**, qui permet de revenir en arrière et répéter une instruction ou un ensemble d'instructions plusieurs fois en boucle;
- Le **sous-programme**, qui permet d'interrompre le cours normal du programme et d'exécuter du code déclaré ailleurs;
- Le **déplacement**, qui permet de se "déplacer" ailleurs dans le code. Ce type de structure de contrôle est jugée désuète dans de nombreux langages de programmation, y compris PowerShell.


### Les structures de sélection

#### Sélection à une branche (*If*)

La structure `If` permet une sélection de code conditionnelle. Dans sa variante à une branche, on pose une condition booléenne; si la condition est vraie, le code spécifié entre accolades sera exécuté, autrement il sera ignoré.

<Tabs>
<TabItem value="code" label="Code">

```powershell
[int] $nombre = Read-Host "Entrez un nombre de 1 à 3..."

if ($nombre -eq 1) {
    Write-Host "Un!"
}

Write-Host "Fini!"

```

</TabItem>
<TabItem value="resultat" label="Résultat">

<PowerShellWindow workdir="C:\Scripts" command=".\If.ps1" result="
Entrez un nombre de 1 à 3...: 1
Un!
Fini!" />

</TabItem>
</Tabs>


#### Sélection à deux branches (*If...Else*)

Dans sa variable à deux branche, on déclare un deuxième bloc de code à l'aide de l'instruction `else`. C'est le code qui sera exécuté uniquement si la condition est évaluée `false`.


<Tabs>
<TabItem value="code" label="Code">

```powershell
[int] $nombre = Read-Host "Entrez un nombre de 1 à 3..."

if ($nombre -eq 1) {
    Write-Host "Un!"
}
else {
    Write-Host "Autre!"
}

Write-Host "Fini!"
```

</TabItem>
<TabItem value="resultat" label="Résultat">

<PowerShellWindow workdir="C:\Scripts" command=".\IfElse.ps1" result="
Entrez un nombre de 1 à 3...: 2
Autre!
Fini!" />

</TabItem>
</Tabs>



#### Sélection imbriquée (*If...ElseIf...Else*)

On peut imbriquer autant de blocs `ElseIf` que l'on veut entre le `If` et le `Else`. Chaque bloc `ElseIf` pose une condition. Si la condition spécifiée dans le bloc `If` ou dans le bloc `ElseIf` précédent est fausse, alors le bloc suivant tente son exécution. En dernier recours, le bloc `Else` est exécuté uniquement lorsque toutes les conditions des blocs `If` et `ElseIf` n'ont pas été respectées. Il est important de noter que dès qu'une condition est vraie, les blocs `ElseIf` suivants ne seront pas exécutés, même si leur condition est vraie; seulement le premier à évaluer vrai sera exécuté.

<Tabs>
<TabItem value="code" label="Code">

```powershell
[int] $nombre = Read-Host "Entrez un nombre de 1 à 3..."

if ($nombre -eq 1) {
    Write-Host "Un!"
}
elseif ($nombre -eq 2) {
    Write-Host "Deux!"
}
else { 
    Write-Host "Autre!" 
}

Write-Host "Fini!"
```

</TabItem>
<TabItem value="resultat" label="Résultat">

<PowerShellWindow workdir="C:\Scripts" command=".\IfElseifElse.ps1" result="
Entrez un nombre de 1 à 3...: 2
Deux!
Fini!" />

</TabItem>
</Tabs>


#### Sélection à *n* branches (*switch*)

L'instruction `Switch` est une alternative à `If`. Elle est pratique lorsqu'on a de nombreuses valeur à tester et que la condition est simple, comme les options d'un menu par exemple.

Optionnellement, on peut spécifier un cas Default, qui est sélectionné si aucun autre cas n'est évalué vrai.

<Tabs>
<TabItem value="code" label="Code">

```powershell
[int] $nombre = Read-Host "Entrez un nombre de 1 à 3..."

switch ($nombre) {
    1   { Write-Host "Un!"      }
    2   { Write-Host "Deux!"    }
    3   { Write-Host "Trois!"   }
    Default { 
        Write-Host "Autre!" 
    }
}

Write-Host "Fini!"
```

</TabItem>
<TabItem value="resultat" label="Résultat">

<PowerShellWindow workdir="C:\Scripts" command=".\Switch.ps1" result="
Entrez un nombre de 1 à 3...: 2
Deux!
Fini!" />

</TabItem>
</Tabs>


#### Exemples de sélection

##### Tester l'existence d'un répertoire

```powershell
if (Test-Path -Path "C:\Minou" -PathType Container) {
    Write-Host "Le répertoire existe!"
}
else {
    Write-Host "Le répertoire n'existe pas!"
}
```

##### Tester si une collection est vide

```powershell
$chemin = Read-Host -Prompt "Entrez un chemin de répertoire"

$documents = Get-ChildItem -Path $chemin | Where-Object { $_.Name -like "*.docx" }

if ($documents.count -gt 0) {
    Write-Host "Il y a $($documents.count) documents."
}
else {
    Write-Host "Il n'y a aucun document."
}
```


##### Tester si un objet est nul (cas particulier)

```powershell
$service = Get-Service "Nexistepas"

if ($null -ne $service) {
    Write-Host "Le service est: $($service.status)."
}
else {
    Write-Host "Service introuvable."
}
```


### Structures de répétition (boucles)

#### Boucle *While*

Dans une boucle *While*, le code entre accolade est exécuté si la condition est vraie. Le code est réexécuté encore et encore tant que l’évaluation de la condition soit toujours vraie. Lorsque la condition devient fausse, la boucle est interrompue et le code continue son exécution normale.

<Tabs>
<TabItem value="code" label="Code">

```powershell
$i = 0

while ($i -le 3) {
    Write-Host $i
    $i++
}

Write-Host "Fini!"
```

</TabItem>
<TabItem value="resultat" label="Résultat">

<PowerShellWindow workdir="C:\Scripts" command=".\While.ps1" result="
0
1
2
3
Fini!" />

</TabItem>
</Tabs>


#### Boucle *Do ... While*

Dans une boucle *Do ... While*, le code entre accolades après l’instruction do est exécuté.
À la fin du bloc de code, si la condition est respectée, ce code est réexécuté.
Lorsque la condition devient fausse, la boucle est interrompue et le code continue son exécution normale.

<Tabs>
<TabItem value="code" label="Code">

```powershell
$i = 0

do {
    Write-Host $i
    $i++
}
while ($i -le 3)

Write-Host "Fini!"
```

</TabItem>
<TabItem value="resultat" label="Résultat">

<PowerShellWindow workdir="C:\Scripts" command=".\DoWhile.ps1" result="
0
1
2
3
Fini!" />

</TabItem>
</Tabs>
