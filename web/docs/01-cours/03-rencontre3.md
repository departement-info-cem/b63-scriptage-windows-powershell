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


Le caractère *Backtick* agit également de caractère d'échappement, c'est-à-dire que tout caractère qui le suit dans une chaîne de caractères est interprété comme un caractère normal, sans égard à son utilité normale.

| Caractère | Hex. | Déc. | Description |
| -- | -- | -- | -- |
| \`\` | 0x60 | 96 | Le caractère *backtick* (accent grave) |
| \`" | 0x22 | 34 | Le guillemet double |
| \`' | 0x27 | 39 | Le guillemet simple |
| \`# | 0x23 | 35 | Le carré (pour marquer les commentaires) |
| \`$ | 0x24 | 36 | Le signe de piastre (pour marquer les variables) |

Finalement, le caractère *Backtick* peut aussi être utilisé pour couper une ligne en deux.

```powershell
Copy-Item -Path "C:\Minou\miaou.txt" `
          -Destination "C:\Pitou\wouf.txt" `
          -Force
```


:::tip

Ce caractère est difficile à trouver sur certains clavier, dont celui qui est le plus commun au Québec. Sur ce clavier (celui où on presse sur la touche de l'accent grave puis sur celle de la lettre), il suffit de faire un "espace accent grave".

![image](./assets/r03/r05_05a.png)

:::


## Visual Studio Code

Les scripts PowerShell sont des fichiers texte, il est donc parfaitement possible de les développer à l'aide de n'importe quel éditeur de texte (le bloc-notes, Notepad++, Nano, etc.)

Celui que nous utiliserons dans ce cours est [**Visual Studio Code**](https://code.visualstudio.com/), un environnement intégré de développement (IDE) gratuit en *open-source* développé par Microsoft. Il possède de nombreuses fonctionnalités pour aider les programmeurs à écrire, gérer et tester leur code. Il supporte de nombreux langages de programmation, dont PowerShell. C'est cet outil de développement que vous utiliserez tout au long de ce cours.

![image](./assets/r03/r05_06a.png)


:::info
Il existe d'autres logiciels que vous êtes libres d'explorer. Vous connaissez peut-être [**Windows PowerShell ISE**](https://learn.microsoft.com/fr-ca/powershell/scripting/windows-powershell/ise/introducing-the-windows-powershell-ise?view=powershell-5.1), qui est inclus dans Windows. Bien qu'il ne soit pas aussi avancé que VS Code sur plusieurs aspects, il peut s'avérer utile lorsque vous devez développer ou modifier un script à la volée sur un serveur, sans vouloir installer un logiciel. Vous pouvez le démarrer par le menu Démarrer, ou encore en lançant la commande `PowerShell_ISE.exe`. Notez cependant que Windows PowerShell ISE ne supporte pas les versions de PowerShell plus récentes; son support se termine à la version 5.1.  Microsoft a cessé d'améliorer ce logiciel au profit de Visual Studio Code, qui est désormais considéré par Microsoft comme l'environnement de développement "officiel" pour PowerShell.

Parmi les autres environnements populaires, il y a [**Idera PowerShell Plus**](https://www.idera.com/productssolutions/freetools/powershellplus/), principalement orienté vers les administrateurs de bases de données, et [**SAPIEN PowerShell Studio**](https://www.sapien.com/software/powershell_studio), qui se démarque par de nombreuses fonctionnalités dont un éditeur d'interfaces graphiques, un compilateur intégré, et plusieurs accélérateurs, mais qui coûte assez cher (250$ US par année ou 25$ US par mois).
:::

### Espace de travail (*workspace*)

Il est idéal de travailler dans VS Code en y ouvrant un dossier. Vous pouvez le faire soit en cliquant sur Ouvrir le dossier dans l'interface de VS Code, soit 

![image](./assets/r03/r05_06b.png)

Dans le panneau de gauche, vous pouvez voir tous vos fichiers et dossiers et les ouvrir dans les onglets. Vous pouvez-même ouvrir plusieurs fichiers côte à côte.

![image](./assets/r03/r05_06c.png)


### Palette de commandes

Vous pouvez ouvrir la palette de commandes en appuyant sur la touche F1 ou Ctrl+Maj+P. Vous pouvez rechercher des fonctionnalités et des actions.

![image](./assets/r03/r05_06d.png)


### Terminal intégré et exécution PowerShell

Lorsque vous avez un fichier .PS1 ouvert, le terminal intégré PowerShell démarre automatiquement.

Vous pouvez utiliser ce terminal de la même manière qu'une invite PowerShell standard.

Si vous sélectionnez une partie de votre code et appuyez sur la touche F8, ce code sera automatiquement passé dans le terminal.

![image](./assets/r03/r05_06e.png)


Pour lancer le script facilement, appuyez simplement sur F5. Cela exécute le script dans le terminal intégré en mode débogage.

![image](./assets/r03/r05_06f.png)


Pour automatiquement sélectionner toutes les occurrences d'un même mot (par exemple, pour changer le nom d'une variable partout à la fois), sélectionner le texte à remplacer puis faites Ctrl+F2 (ou clic droit, Modifier toutes les occurrences).

![image](./assets/r03/r05_06g.png)


### Installation

Dans les laboratoires du Collège, VS Code est déjà installé et configuré pour le développement de scripts en PowerShell. Si vous souhaitez l'utiliser sur votre ordinateur personnel ou sur une machine virtuelle, suivez ces directives.

Téléchargez l'application sur le site officiel (https://code.visualstudio.com/). Il en existe plusieurs variantes:
- Stable build: c'est la version la plus stable, et celle que vous devriez utiliser.
- Insider build: c'est une préversion pour tester les nouvelles fonctionnalités. Il peut y avoir des bogues.

Choisissez l'installateur qui convient à vos besoin.

- User Installer: c'est l'option par défaut. VS Code s'installe dans votre profil utilisateur, donc vous n'avez pas besoin de droits d'administration. Il sera installé seulement pour vous, pas pour les autres utilisateurs.
- System Installer: pour installer VS Code pour tous les utilisateurs du système. Ça prend des droits d'administration.

![image](./assets/r03/r05_06h.png)

Puis, lancez l'installateur et répondez aux questions. Assurez-vous de déposer une icône sur le bureau et d'enregistrer les actions au menu contextuel.

![image](./assets/r03/r05_06i.png)

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

