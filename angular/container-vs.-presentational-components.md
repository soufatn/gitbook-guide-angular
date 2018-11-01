# Container vs. Presentational Components

Supposons que nous disposons dans notre composant `app`, d'une liste `bookList` contenant une liste d'instance d'une classe `Book` que nous souhaitons afficher.


```typescript
export class Book {

    title?: string;

    constructor(args: Book = {}) {
        this.title = args.title;
    }

}
```

```typescript
import { Book } from './book/book';

...
export class AppComponent {
    bookList = [
        new Book({
            title: 'eXtreme Programming Explained'
        }),
        new Book({
            title: 'ReWork'
        })
    ];
};
```


Laissez bien sûr l'IDE s'occuper des imports via l'auto-complete ou l'auto-import _\(Alt + Enter chez JetBrains\)_.


![IntelliJ Class Completion](../.gitbook/assets/intellij-class-completion.gif)

![IntelliJ Auto Import](../.gitbook/assets/intellij-auto-import.gif)

Il serait intéressant de déléguer l'affichage de chaque `book` au composant `book-preview` que nous pourrons réutiliser plus tard dans d'autres contextes.

Dans ce cas, nous séparons les responsabilités entre le composant `app` et le composant `book-preview`. 


```markup
<!-- Display one book-preview component for each book... -->
<!-- ...but we need to find some way to pass the book to each component. -->
<wt-book-preview
        *ngFor="let book of bookList"></wt-book-preview>
```


## Container Component _\(ou Smart Component\)_

Le composant `app` **s'occupe donc de la "business logic"** et sélectionne les objets `book` à afficher via la propriété `bookList`.  
Il est donc un "Container Component" qui délègue l'affichage à des "Presentational Components".

## Presentational Component _\(ou Dumb Component\)_

Le composant `book-preview` ne sait pas d'où provient le `book` à afficher mais il sait l'afficher.  
Il est donc un "Presentational Component" qui est débarrassé de la "business logic".

## Article plus détaillé sur le sujet

[https://blog.wishtack.com/2017/05/05/the-guide-to-building-quality-angular-2-components/](https://blog.wishtack.com/2017/05/05/the-guide-to-building-quality-angular-2-components/)

## Interaction entre Composants

Il ne reste plus au composant app qu'à communiquer chaque `book` au composant `book-preview` associé. Cf. [Interaction entre Composants](interaction-entre-composants/).

