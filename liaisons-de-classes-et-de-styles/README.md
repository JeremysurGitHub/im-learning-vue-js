# Liaisons de classes et de styles

## Sommaire

* Description : Un besoin classique de la liaison de données est la manipulation de la liste des classes d’un élément, ainsi que ses styles en ligne. Vue fournit des améliorations spécifiques quand v-bind est utilisé avec class et style. En plus des chaines de caractères, l’expression peut évaluer des objets ou des tableaux.

* Liaison de Classes HTML

    * Syntaxe Objet : Il est possible de passer un objet à v-bind:class pour permuter les classes automatiquement. Vous pouvez permuter plusieurs classes en ayant plus de champs dans l’objet. De plus, la directive v-bind:class peut aussi coexister avec l’attribut class.

    * Syntaxe Tableau : Il est possible de passer un tableau à v-bind:class pour appliquer une liste de classes. Si vous voulez permuter une classe de la liste en fonction d’une condition, vous pouvez le faire avec une expression ternaire. Il est aussi possible d’utiliser la syntaxe objet dans la syntaxe tableau.

    * Avec des Composants : Cette section suppose la connaissance de Vue Composants. N’hésitez pas à l’ignorer et y revenir plus tard.

* Liaison de Styles HTML

    * Syntaxe objet : La syntaxe objet pour v-bind:style est assez simple - cela ressemble presque à du CSS, sauf que c’est un objet JavaScript. C’est souvent une bonne idée de lier directement un objet de style, pour que le template soit plus propre. Encore une fois, la syntaxe objet est souvent utilisée en conjonction avec des propriétés calculées retournant des objets.
 
    * Syntaxe Tableau : La syntaxe tableau pour v-bind:style permet d’appliquer plusieurs objets de style à un même élément.

    * Préfixage automatique : Quand vous utilisez une propriété CSS qui nécessite un préfixe vendeur dans v-bind:style, par exemple transform, Vue le détectera automatiquement, et ajoutera les préfixes appropriés aux styles appliqués.

    * Valeurs multiples : Introduit dans la 2.3.0+, vous pouvez fournir de multiples valeurs de préfixes à une propriété style. Ceci rendra uniquement la dernière valeur dans le tableau supportée par le navigateur. 

## Description

Un besoin classique de la liaison de données est la manipulation de la liste des classes d’un élément, ainsi que ses styles en ligne. Puisque ce sont tous deux des attributs, il est possible d’utiliser v-bind pour les gérer : il faut simplement générer une chaine de caractère avec nos expressions. Cependant la concaténation de chaine de caractères est fastidieuse et source d’erreur. Pour cette raison, Vue fournit des améliorations spécifiques quand v-bind est utilisé avec class et style. En plus des chaines de caractères, l’expression peut évaluer des objets ou des tableaux.

## Liaison de Classes HTML

### Syntaxe Objet

Il est possible de passer un objet à v-bind:class pour permuter les classes automatiquement :

```javascript
<div v-bind:class="{ active: isActive }"></div>
```

La syntaxe ci-dessus signifie que la classe active sera présente si la propriété isActive est considérée comme vraie.
Vous pouvez permuter plusieurs classes en ayant plus de champs dans l’objet. De plus, la directive v-bind:class peut aussi coexister avec l’attribut class. Donc, avec le template suivant :

```javascript
<div
  class="static"
  v-bind:class="{ active: isActive, 'text-danger': hasError }"
></div>
```

Et les données suivantes :

```javascript
data: {
  isActive: true,
  hasError: false
}
```

Le rendu sera :

```javascript
<div class="static active"></div>
```

Quand isActive ou hasError change, la liste des classes sera mise à jour en conséquence. Par exemple, si hasError devient true, la liste des classes deviendra "static active text-danger".
L’objet lié n’a pas besoin d’être déclaré dans l’attribut :

```javascript
<div v-bind:class="classObject"></div>
```

```javascript
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```

Ceci rendra le même résultat. Il est également possible de lier une propriété calculée qui retourne un objet. C’est une méthode courante et puissante :

```javascript
<div v-bind:class="classObject"></div>
```

```javascript
data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```

### Syntaxe Tableau

Il est possible de passer un tableau à v-bind:class pour appliquer une liste de classes :

```javascript
<div v-bind:class="[activeClass, errorClass]"></div>
```

```javascript
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```

Ce qui rendra :

```javascript
<div class="active text-danger"></div>
```

Si vous voulez permuter une classe de la liste en fonction d’une condition, vous pouvez le faire avec une expression ternaire :

```javascript
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
```

Ceci appliquera toujours la classe errorClass, mais appliquera activeClass uniquement quand isActive vaut true.
En revanche, cela peut être un peu verbeux si vous avez plusieurs classes conditionnelles. C’est pourquoi il est aussi possible d’utiliser la syntaxe objet dans la syntaxe tableau :

```javascript
<div v-bind:class="[{ active: isActive }, errorClass]"></div>
```

### Avec des Composants

> Cette section suppose la connaissance de Vue Composants. N’hésitez pas à l’ignorer et y revenir plus tard.

## Liaison de Styles HTML

### Syntaxe Objet

La syntaxe objet pour v-bind:style est assez simple - cela ressemble presque à du CSS, sauf que c’est un objet JavaScript. Vous pouvez utiliser camelCase ou kebab-case (utilisez des apostrophes avec kebab-case) pour les noms des propriétés CSS :

```javascript
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```

```javascript
data: {
  activeColor: 'red',
  fontSize: 30
}
```

C’est souvent une bonne idée de lier directement un objet de style, pour que le template soit plus propre :

```javascript
<div v-bind:style="styleObject"></div>
```

```javascript
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```

Encore une fois, la syntaxe objet est souvent utilisée en conjonction avec des propriétés calculées retournant des objets.

### Syntaxe Tableau

La syntaxe tableau pour v-bind:style permet d’appliquer plusieurs objets de style à un même élément.

```javascript
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```

### Préfixage automatique

Quand vous utilisez une propriété CSS qui nécessite un préfixe vendeur dans v-bind:style, par exemple transform, Vue le détectera automatiquement, et ajoutera les préfixes appropriés aux styles appliqués.

### Valeurs multiples

> 2.3.0+

Introduit dans la 2.3.0+, vous pouvez fournir de multiples valeurs de préfixes à une propriété style, par exemple :

```javascript
<div v-bind:style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```

Ceci rendra uniquement la dernière valeur dans le tableau supportée par le navigateur. Dans cet exemple, cela rendra display: flex pour les navigateurs qui supportent la version sans préfixe de flexbox.