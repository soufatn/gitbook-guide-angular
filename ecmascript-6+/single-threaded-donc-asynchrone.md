# "Single-Threaded" donc Asynchrone

## Multi-thread vs mono-thread

Dans un univers data-driven synchrone et "multi-threaded"...

```javascript
function processRequest(request) {

    /*
     * Pre-processing.
     */
    var query = prepareQuery(...);

    var result = execQuery(query); // might take few seconds...

    /*
     * Post-processing.
     */
    var response = createResponse(result);

    return response;

}
```

...mais dans un univers "single-threaded" cela peut être catastrophique car l'application est bloquée tant qu'elle est en attente.

Tant qu'on ne sort pas de la fonction, aucune autre fonction ne peut être exécutée.

## Besoin de traitement asynchrone

```javascript
function processRequest(request, callback) {

    /*
     * Pre-processing.
     */
    var query = prepareQuery(...);

    execQuery(query, function (result) {

        /*
         * Post-processing.
         */
        var response = createResponse(result);

        callback(response);

    });

}
```

## Avantages de l'approche asynchrone

* Pas de limitation due au nombre de threads.
* Pas de locks ou sémaphores.
* Pas de locks gourmants.
* Pas de deadlock.
* Les données ne peuvent pas varier lors de l'exécution d'une fonction synchrone.

## Closure

Un "closure" est la combinaison d'une définition de fonction ainsi que l'environnement lexical dans lequel cette dernière est définie.  
Les "closures" déterminent la portée des variables.

Les variables sont accessibles en lecture / écriture dans le "closure" où elles sont déclarées mais également dans les fonctions définies à l'intérieur.

```javascript
function main() {

    var userName = 'Foo';

    function setUserName(value) {
        userName = value;
    }

    function getUserName() {
        return userName;
    }

    setUserName('John');

    console.log(getUserName()); // 'John';

}

main();
```

## Event Loop

### Comportement de l'Event Loop

Quel est l'ordre d'exécution ?

```javascript
var value;

setTimeout(function () {
    value = 'VALUE';
}, 100 /* 100 ms. */);

console.log(value); // ???

setTimeout(function () {
    console.log(value); // ???
}, 200);
```

```javascript
var value;

setTimeout(function () {
    value = 'VALUE';
}, 100 /* 100 ms. */);

console.log(value); // 1 - undefined

setTimeout(function () {
    console.log(value); // 2 - VALUE
}, 200);
```

Et dans ce cas ?


```javascript
function main() {

    var value;

    setTimeout(function () {
        value = 'VALUE';
    }, 0 /* 0 ms. */);

    console.log(value); // ???

    setTimeout(function () {
        console.log(value); // ???
    }, 0);
    
    console.log(value); // ???

}

main();
```

```javascript
function main() {

    var value;

    setTimeout(function () {
        value = 'VALUE';
    }, 0 /* 0 ms. */);

    console.log(value); // 1 - undefined

    setTimeout(function () {
        console.log(value); // 3 - VALUE
    }, 0);
    
    console.log(value); // 2 - undefined
    
}

main();
```


### Fonctionnement de l'Event Loop

1. **Inscription d'un listener** : Tous les traitements asynchrones nécessitent la définition d'une fonction de callback afin de récupérer le résultat du traitement ou simplement savoir que le traitement a abouti. Lors de cette étape, nous inscrivons explicitement ou indirectement un listener \(notre callback\) sur un événement. Exemples: `setTimeout`, `addEventListener` etc... 
2. **Ajout de la fonction à la queue** : Cette fonction de callback ne peut pas être appelée immédiatement dès réception du résultat car le thread est probablement occupé par l'exécution d'une autre fonction. Dans notre cas, nos deux fonctions de callback sont prêtes à être appelées mais le thread est occupé par l'exécution de notre fonction `main`. Le moteur JavaScript ajoute donc les fonctions de callback à la fin d'une "queue" de fonctions à appeler quand le thread sera libre. 
3. **Tick** : A la fin de l'exécution de la fonction `main`, le thread n'a pas le temps de s'ennuyer et va récupérer la fonction en tête de "queue" pour l'exécuter. Plus précisément, il s'agit ici de l'"event loop" qui est simplement la boucle infinie qui lance les fonctions les unes après les autres. **L'instant où l'"event loop" récupère une nouvelle fonction à exécuter s'appelle un "tick".**

![Event Loop](../.gitbook/assets/event-loop.jpg)


[https://www.youtube.com/watch?v=8aGhZQkoFbQ](https://www.youtube.com/watch?v=8aGhZQkoFbQ)







