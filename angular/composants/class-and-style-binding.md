# Class & Style Binding

## Class Binding

```markup
<div [class.wt-important]="isImportant">Important Stuff</div>
```


Evitez la construction manuelle de la "string" class :  
`classString += ' b'` puis `<div [class]="classString"></div>`.


[https://angular.io/guide/template-syntax\#class-binding](https://angular.io/guide/template-syntax#class-binding)



## Style Binding

```markup
<button [style.background-color]="isValid ? null : 'red'">Save</button>
```

```markup
<button [style.font-size.em]="isImportant ? 2 : 1" >Big</button>
```

[https://angular.io/guide/template-syntax\#style-binding](https://angular.io/guide/template-syntax#style-binding)





