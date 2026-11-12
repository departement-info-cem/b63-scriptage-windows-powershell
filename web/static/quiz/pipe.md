---
titre: Quiz - Pipeline!
duree: 15
---

## Question 1 – Quel est le rôle du symbole `|` dans la commande suivante ?

```
Get-Service | Out-Null
```


- [x] Transmettre les objets produits par `Get-Service` à `Out-Null`.
- [ ] Afficher les services avant d'exécuter `Out-Null`.
- [ ] Exécuter `Out-Null` avant `Get-Service`.
- [ ] Enregistrer automatiquement les services dans une variable.


## Question 2 – Que se passe-t-il avec les objets produits par la commande suivante ?

```
Get-Process
```


- [ ] Ils sont automatiquement enregistrés dans une variable.
- [x] Ils arrivent dans la console et sont convertis en texte pour être affichés.
- [ ] Ils sont automatiquement supprimés par `Out-Null`.
- [ ] Ils sont conservés dans le pipeline jusqu'à la prochaine commande.


## Question 3 – Quelle est l'instruction qui permet d'éviter l'affichage direct des objets produits par `Get-Service` dans la console ?

Sélectionnez les deux bonnes réponses.


- [x] `$services = Get-Service` et `Get-Service | Out-Null`
- [ ] `Get-Service`
- [ ] `Out-Null | Get-Service`


## Question 4 – Une commande PowerShell peut recevoir un ou plusieurs objets en entrée, effectuer un traitement et produire un objet en sortie.



- [x] Vrai
- [ ] Faux


## Question 5 – L'exécution de la commande suivante arrête les processus Windows retournés par `Get-Process`.

```
Get-Process | Out-Null
```


- [ ] Vrai
- [x] Faux


## Question 6 – On exécute les deux instructions suivantes :


```
$services = Get-Service
$services
```

Quel est le résultat de la deuxième instruction ?


- [x] Elle affiche les services enregistrés dans la variable.
- [ ] Elle exécute une nouvelle fois la commande `Get-Service`.
- [ ] Elle supprime les services de Windows.
- [ ] Elle affiche uniquement le nombre de services.


## Question 7 – On exécute la commande suivante dans une session où la variable `$services` n'existe pas encore :



```
$services = Get-Service | Out-Null
```

Que contiendra la variable `$services` après l'exécution ?


- [ ] La liste complète des services Windows.
- [ ] Le nombre de services Windows.
- [x] La valeur `$null`.
- [ ] Le texte affiché par la commande `Get-Service`.


## Question 8 – Un étudiant exécute les trois instructions suivantes, l'une après l'autre :



```
Get-Service | Out-Null
Get-Process | Out-Null
Get-Service
```

Qu'est-ce qui sera affiché dans la console ?


- [ ] Les services et les processus Windows.
- [ ] Uniquement les processus Windows.
- [x] Uniquement les services Windows retournés par la troisième instruction.
- [ ] Rien ne sera affiché.


## Question 9 – Un étudiant exécute :


```
Get-Service | Out-Null
```

Il affirme que `Get-Service` n'est pas réellement exécutée puisque son résultat n'est pas affiché.

Quelle affirmation est correcte ?


- [ ] `Get-Service` est ignorée parce que `Out-Null` bloque son exécution.
- [x] `Get-Service` est exécutée, mais les objets produits sont éliminés par `Out-Null`.
- [ ] `Get-Service` est exécutée uniquement si des services sont en cours d'exécution.
- [ ] `Out-Null` remplace complètement l'exécution de `Get-Service`.

