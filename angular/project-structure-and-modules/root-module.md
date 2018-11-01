# Root Module

Une application Angular contient un seul et unique "root module" _\(`AppModule` par défaut\)_.

Le "root module" est un module classique dont la particularité est de définir le "root component" de l'application via la propriété `bootstrap`.


```typescript
@NgModule({

    declarations: [
        AppComponent
    ],
    imports: [
        BookModule
    ],
    bootstrap: [
        AppComponent
    ]
})
export class AppModule {
}
```


> `bootstrap` est une liste car dans certains cas extrêmes, il est possible d'avoir plusieurs "root components".

> Une alternative à la propriété `bootstrap` est de surcharger la méthode `ngDoBootstrap`.

Le module `AppModule` est désigné comme "root module" via la ligne suivante du fichier `main.ts`.


```typescript
import { AppModule } from './app/app.module';

platformBrowserDynamic().bootstrapModule(AppModule);
```


Au démarrage de l'application, Angular recherche dans le DOM _\(Cf. `src/index.html`\)_, **le premier élément** correspondant au **sélecteur du composant** `AppComponent` _\(`wt-app`\)_ et injecte alors le composant à cet endroit.

