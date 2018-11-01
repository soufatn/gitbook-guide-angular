# \*ngIf

Alors que le [template interpolation](template-interpolation.md) et le [property binding](property-binding.md) permettent de modifier l'affichage et le contenu, ils ne permettent pas de modifier la structure du DOM en ajoutant ou en retirant des éléments par exemple.

Pour remédier à cette limitation, Angular fournit des **directives structurelles** qui permettent de modifier la structure du DOM.

L'une de ces directives les plus utilisées est le `ngIf`.

Si l'expression associée à la directive est "falsy" alors l'élément et son contenu sont retirés du DOM _\(ou jamais ajoutés\)._


```markup
<button *ngIf="isAvailable">BUY</button>
```

```typescript
...
export class AppComponent {
    isAvailable = false;
}
```


## \*ngIf vs "safe navigation operator"

Les expressions utilisées à l'intérieur ne sont donc jamais évaluées et dans certains cas cela évite certaines erreurs.



```markup
<div>
    <span>{{ book.name }}</span>
</div>
```


```typescript
...
export class AppComponent {
    book = null;
}
```



```text
TypeError: Cannot read property 'name' of undefined
```

Il suffit alors d'utiliser `ngIf` pour retirer tout le bloc _\(si cela est nécessaire\)_.


Si le cas ne doit jamais se produire alors il vaut mieux déclencher une erreur bien bruyante plutôt que de camoufler le problème.



```markup
<div *ngIf="book">
    <span>{{ book.name }}</span>
</div>
```


Angular propose également une opérateur de "safe navigation" qu'il vaut mieux éviter car les éléments sont présents dans le DOM mais vides et cela peut avoir des impacts sur l'affichage et le styling.


```markup
<div>
    <span>{{ book?.name }}</span>
</div>
```


## `switch / case` & `if / then / else`

Angular propose des directives pour implémenter des "`switch / case`" et des `"if / then / else`" dans les templates.

[https://angular.io/api/common/NgIf](https://angular.io/api/common/NgIf)  
[https://angular.io/api/common/NgSwitch](https://angular.io/api/common/NgSwitch)


Nous vous recommandons d'éviter leur utilisation pour des raisons de lisibilité et de maintenabilité.

Pour répondre à ce genre de besoins, Angular propose des solutions bien plus élégantes avec l'injection dynamique de composants par exemple _\(sujet abordé plus tard dans ce guide\)_.

Chez Wishtack, nous considérons l'usage de "`if / else`" et de "`switch / case`" comme des codes smells peu importe le langage. Leur utilisation dans des templates est un code smell encore plus fort.


