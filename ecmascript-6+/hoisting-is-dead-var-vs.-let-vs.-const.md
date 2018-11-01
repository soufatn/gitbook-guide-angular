# Hoisting is Dead: var vs. let vs. const

## Rappel

### Variables globales 🤮

```javascript
userName = 'Foo BAR';

console.log(userName); // Foo BAR
```

### Use strict 😅

```javascript
'use strict';

userName = 'Foo BAR'; // ReferenceError: userName is not defined
```

```javascript
'use strict';

console.log(userName); // ReferenceError: userName is not defined
```

## Hoisting

### Variable hoisting


```javascript
'use strict';

console.log(userName); // ???

var userName = 'Foo BAR';
```


```javascript
'use strict';

console.log(userName); // undefined

var userName = 'Foo BAR';
```


### Function hoisting


```javascript
'use strict';

greetings(); // ???

function greetings() {
    console.log('HI!');
}

function greetings() {
    console.log('HELLO!');
}
```


```javascript
'use strict';

greetings(); // HELLO!

function greetings() {
    console.log('HI!');
}

function greetings() {
    console.log('HELLO!');
}
```


### Un peu mieux

```javascript
'use strict';

greetings(); // TypeError: greetings is not a function.

var greetings = function () {
    console.log('HI!');
};

greetings(); // HI!

var greetings = function () {
    console.log('HELLO!');
};

greetings(); // HELLO!
```

## let

Les variables ne sont accessibles qu'après leur déclaration.

```javascript
console.log(userName); // ReferenceError: userName is not defined

let userName = 'Foo BAR';
```

Les variables ne sont accessibles que dans le bloc de code.

```javascript
if (true) {
    let userName = 'Foo BAR';
}

console.log(userName); // ReferenceError: userName is not defined
```

## const

`const` permet de déclarer des variables constantes qui ne peuvent pas être réinitialisées.

```javascript
const user = {
    firstName: 'Foo',
    lastName: 'BAR'
}

user = null; // TypeError: Assignment to constant variable.
```


`const` n'est pas immutable.

```javascript
user.firstName = 'John'; // OK!
```


Il est recommandé de déclarer toutes les variables en `const` sauf quand la réutilisation d'une variable s'avère inévitable.

Cela évite de nombreuses erreurs d'inattention difficiles à diagnostiquer.

L'utilisation de const décourage le recyclage maladroit de variables :

```javascript
const user = {
    firstName: 'Foo',
    lastName: 'BAR'
}

/* 🤢*/
user = user.firstName; // TypeError: Assignment to constant variable.
```


