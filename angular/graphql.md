# GraphQL

## Apollo Angular

La solution la plus commune pour consommer des APIs GraphQL est le package Apollo Angular `apollo-angular`.

### Configuration

Profitez des schematics d'`apollo-angular` !

```bash
yarn ng add apollo-angular
```

A défaut de disposer d'un mécanisme de configuration dynamique de votre application Angular, mettez en place l'URI de votre API GraphQL via les environnements Angular _\(fichiers `environments/environment*.ts`\)_

### Plugin IntelliJ / WebStorm

Le plugin GraphQL d'IntelliJ / WebStorm facilite la consommation d'APIs GraphQL grâce à la découverte automatique du schéma puis l'autocomplétion.

[https://plugins.jetbrains.com/plugin/8097-js-graphql](https://plugins.jetbrains.com/plugin/8097-js-graphql)



## Services Query / Mutate


Bien qu'il soit possible d'utiliser directement les méthodes `query`, `mutate` et `watchQuery` du service `Apollo` pour exécuter des _queries_ et _mutations_, depuis n'importe quel endroit de votre application, **il est préférable d'utiliser des services dédiés à chaque `query` ou `mutation`** qui héritent respectivement des classes `Query` et `Mutation`.


```typescript
@Injectable({
    providedIn: 'root'
})
export class UserListQuery extends Query<{ users: User[] }> {

    document = gql`query UserList {
        users {
            id
            firstName
            lastName
        }
    }`;

}
```

Ce service peut ensuite être utilisé ainsi :

```typescript
this._userListQuery.fetch()
    .subscribe(({data}) => this.userList = data.users);
```


Le service dispose également d'une méthode watch qui vous permet d'observer les changements en cas d'activation du polling _\(option `pollInterval`\)_ ou encore en cas de mise à jour du cache local par exemple.


Dans tous les cas, n'oubliez pas d'unsubscribe de l'`Observable` retourné ou d'utiliser RxScavenger.  
[https://github.com/wishtack/wishtack-steroids/tree/master/packages/rx-scavenger](https://github.com/wishtack/wishtack-steroids/tree/master/packages/rx-scavenger)


## Factorisation des queries grâce au fragments

Pensez à utiliser les fragments pour factoriser vos queries.

[https://www.apollographql.com/docs/angular/features/fragments.html](https://www.apollographql.com/docs/angular/features/fragments.html)



