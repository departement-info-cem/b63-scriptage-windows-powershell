---
title: 8 - Examen formatif
slug: "8"
draft: false
hide_table_of_contents: true
---

Cet examen est une **pratique** pour l'examen de mi-session, qui compte pour **35%** de la note finale du cours (soit **70%** du seuil des examens).


## Contexte de réalisation

Vous avez **3 heures** pour le terminer.

Ce qui est permis pendant l'examen:
- Une feuille de notes personnelles 8&frac12;x11 manuscrite recto-verso
- L'accès au site du cours
- Les fichiers PowerPoint fournis par le professeur
- Les terminaux PowerShell et Windows PowerShell
- Visual Studio Code ainsi que le débogueur

**Toute autre documentation est interdite.**

Puisqu'il s'agit d'un examen de pratique, il sera possible de poser des questions au professeur. N'utilisez ce privilège que lorsque vous êtes vraiment bloqué sur une question.


## Conseils pour la révision

Pour vous permettre de réussir l'examen, vous devez être à l'aise avec les concepts suivants:

- Le format de fichier d'un script PowerShell (.PS1)
- Les conditions d'exécution d'un script:
  - [Stratégie d'exécution](/notions/scripts/execution#stratégie-dexécution-executionpolicy) (*ExecutionPolicy*)
  - [Fichiers bloqués](/notions/scripts/execution#fichiers-bloqués) (*ZoneIdentifier*) et la commande `Unblock-File`
- L'[exécution de scripts](/notions/scripts/execution#exécution-de-scripts)
  - Par l'explorateur de fichiers ou le bureau
  - Par la console PowerShell
  - Par l'invite de commande classique avec l'interpréteur `powershell.exe`
- Les [structures de contrôle](/notions/scripts/controle) et l'algorithmie
  - Les structures de sélection (if..elseif..else, switch)
  - Les structures de répétition (while, do...while, foreach, ForEach-Object) 
- Les [fonctions](/notions/scripts/fonctions)
  - La différence entre la déclaration d'une fonction et l'appel d'une fonction
  - Sortie dans le pipeline
  - L'instruction Return
- Les [paramètres](/notions/scripts/parametres)
  - Paramètres d'un script et paramètres d'une fonction
  - Déclaration de paramètres avec le bloc `Param()`
  - Validation des paramètres (`ValidateSet`, `ValidateLength`, `ValidateScript`, etc.)
  - Paramètres obligatoires
  - Paramètres booléens de type `[Switch]`
  - Passage d'un tableau en paramètre
  - Passage d'un paramètre à l'entrée du pipeline
- Les [fichiers](/notions/scripts/fichiers)
  - Lecture et écriture de fichiers texte
  - Lecture et écriture de fichiers CSV
  - Journalisation (*logging*)
- Domaine Active Directory 
  - [Gestion des utilisateurs et groupes](/notions/adds/comptes) (commandes `ADUser`, `ADGroup`, `ADGroupMember`)
  - [Gestion des unités d'organisation](/notions/adds/administration#unités-dorganisation) (commandes `ADOrganizationalUnit`)
  - [Informations sur le domaine](/notions/adds/administration#informations-sur-le-domaine-et-la-forêt) (`Get-ADDomain`, `Get-ADForest`, `Get-ADRootDSE`)
- [Splatting](/notions/powershell/dictionnaires#splatting)
- [Bonnes pratiques de scripting](/notions/scripts/bonnespratiques)
  - Éviter les alias et les noms de paramètres
  - Commenter son code
  - Indenter son code

