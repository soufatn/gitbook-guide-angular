# Services & Providers

## Déclaration d'un Service

Pour déclarer un service Angular, il suffit de créer une classe TypeScript et de la décorer avec le décorateur `@Injectable()`.


```typescript
@Injectable()
export class BookRepository {
    ...
}
```


N'oubliez pas les parenthèses du décorateur `@Injectable()`.


> Pensez à utiliser un template ou live template dans votre IDE ou encore Angular CLI :  
> `yarn ng generate module book-repository`


Evitez de suffixer tous vos services et leurs fichiers respectivement avec les suffixes `Service` et `.service.ts`. tant qu'il n'y a pas de conflit ou d'ambiguïté.

En suffixant toutes les classes et instances par `Service`, on finit par perdre en lisibilité.

Une classe _\(de type "helper" par exemple\)_ peut devenir un "service" ou cesser d'être un "service" du jour au lendemain, la frontière est fine. 


La bonne pratique est de **toujours ajouter le décorateur** `@Injectable()` bien que celui-ci ne soit actuellement pas obligatoire tant que le service n'a pas de dépendances.


En essayant de l'injecter, 


```typescript
@Component({...})
export class BookPreviewComponent {
    constructor(private _bookRepository: BookRepository) {
    }
}
```


... vous remarquerez l'erreur suivante :

```text
StaticInjectorError(AppModule)[BookPreviewComponent -> BookRepository]: 
  StaticInjectorError(Platform: core)[BookPreviewComponent -> BookRepository]: 
    NullInjectorError: No provider for BookRepository!
```

Angular essaie donc d'injecter une instance de `BookRepository` mais ne sait pas la produire.

## Définition d'un Provider

Afin de pouvoir instancier un service, **Angular a besoin d'un "provider"** lui indiquant **comment produire l'instance** de ce service.

Cela se fait généralement via la **propriété `providers` de la configuration du module associé** mais nous verrons d'autres façons de faire plus tard. Cf. [Portée des Services](portee-des-services.md) et [Tree-Shakable Services](tree-shakable-services.md).

### `useClass`

La façon la plus commune de définir un provider est la suivante :


```typescript
@NgModule({
    providers: [
        {
            provider: BookRepository,
            useClass: BookRepository
        }
    ]
})
export class BookCore
Module {
}
```


Cela veut dire que pour fournir une instance de la classe `BookRepository`, il suffit de l'instancier en lui passant en paramètre les dépendances dont elle a besoin. Cf. [Injection d'un Service Angular](injection-dun-service-angular.md).

Cette approche étant la plus commune, il est alors recommandé d'utiliser la syntaxe raccourcie suivante :


```typescript
@NgModule({
    providers: [
        BookRepository
    ]
})
export class BookCoreModule {
}
```


### `useValue`

Utilisation d'une valeur "hardcoded".


```typescript
@NgModule({
    providers: [
        {
            provider: BookRepository,
            useValue: new FakeBookRepository()
        }
    ]
})
export class BookCoreModule {
}
```


Il est préférable d'éviter cette utilisation.

Dans le cas d'une classe, cela voudrait dire que l'objet est instancié avant le démarrage de l'application et pourrait ne jamais être utilisé.

Dans le cas d'une constante, cf. [Class vs Injection Token](class-vs-injection-token.md).


### `useFactory`

Comme son nom l'indique, cette propriété permet de définir l'instanciation du service via une fonction.


```typescript
@NgModule({
    providers: [
        {
            provider: BookRepository,
            useFactory: () => {
            
                /* Useful for A/B testing. */
                if (isBTeam) {
                    return new BookRepositoryV2();
                }
                
                return new BookRepository();
                
            }
        }
    ]
})
export class BookCoreModule {
}
```




