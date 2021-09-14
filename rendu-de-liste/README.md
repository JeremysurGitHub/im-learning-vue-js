# Rendu de liste

## Sommaire

## Associer un tableau à des éléments avec v-for

Nous pouvons utiliser la directive v-for pour faire le rendu d’une liste d’éléments en nous basant sur un tableau. La directive v-for utilise une syntaxe spécifique de la forme item in items, où items représente le tableau source des données et où item est un <strong>alias</strong> représentant l’élément du tableau en cours d’itération :

```javascript
<ul id="example-1">
  <li v-for="item in items" :key="item.message">
    {{ item.message }}
  </li>
</ul>
```

```javascript
var example1 = new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```

Résultat : https://fr.vuejs.org/v2/guide/list.html

À l’intérieur des structures v-for, nous avons un accès complet aux propriétés de la portée parente. v-for supporte également un second argument optionnel représentant l’index de l’élément courant.

```javascript
<ul id="example-2">
  <li v-for="(item, index) in items">
    {{ parentMessage }} - {{ index }} - {{ item.message }}
  </li>
</ul>
```

```javascript
var example2 = new Vue({
  el: '#example-2',
  data: {
    parentMessage: 'Parent',
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```

Résultat : https://fr.vuejs.org/v2/guide/list.html

Vous pouvez également utiliser of en tant que mot-clé à la place de in pour être plus proche de la syntaxe JavaScript concernant l’utilisation des itérateurs :

```javascript
<div v-for="item of items"></div>
```

## v-for avec l’objet

Vous pouvez aussi utiliser v-for pour itérer sur les propriétés d’un objet.

```javascript
<ul id="v-for-object" class="demo">
  <li v-for="value in object">
    {{ value }}
  </li>
</ul>

new Vue({
  el: '#v-for-object',
  data: {
    object: {
      title: 'How to do lists in Vue',
      author: 'Jane Doe',
      publishedAt: '2016-04-10'
    }
  }
})
```

Vous pouvez également fournir un deuxième argument représentant le nom de la propriété courante (c.-à-d. la clé) :

```javascript
<div v-for="(value, name) in object">
  {{ name }}: {{ value }}
</div>
```

Et un autre pour l’index :

```javascript
<div v-for="(value, name, index) in object">
  {{ index }}. {{ name }}: {{ value }}
</div>
```

Résultat : https://fr.vuejs.org/v2/guide/list.html

> Quand vous itérez sur un objet, l’ordre est basé sur l’ordre d’énumération de Object.keys() et il n’y a aucune garantie de cohérence à travers toutes les implémentations des moteurs JavaScript.

## Maintaining State

Quand Vue met à jour une liste d’éléments rendus avec v-for, il utilise par défaut une stratégie de « modification localisée » (in-place patch). Si l’ordre des éléments d’un tableau dans data a changé, plutôt que de déplacer les éléments du DOM pour concorder avec le nouvel ordre des éléments, Vue va simplement modifier chaque élément déjà en place et s’assurer que cela reflète ce qui aurait dû être rendu à cet index en particulier. C’est un comportement similaire au track-by="$index" de Vue 1.x.

Ce mode par défaut est performant, mais adapté seulement lorsque le résultat du rendu de votre liste ne dépend pas de l’état d’un composant enfant ou de l’état temporaire du DOM (par ex. : les valeurs de champs d’un formulaire).

Pour expliquer à Vue comment suivre l’identité de chaque nœud, afin que les éléments existants puissent être réutilisés et réordonnés, vous devez fournir un attribut unique key pour chaque élément :

```javascript
<div v-for="item in items" :key="item.id">
  <!-- contenu -->
</div>
```

Il est recommandé de fournir une key avec v-for chaque fois que possible, à moins que le contenu itéré du DOM soit simple ou que vous utilisiez intentionnellement le comportement de base pour un gain de performance.

Comme c’est un mécanisme générique pour Vue permettant d’identifier les nœuds, la key a également d’autres usages et ne se limite pas seulement à son utilisation avec v-for, comme nous le verrons plus tard dans le guide.

> N’utilisez pas des valeurs non primitive comme des objets ou des tableaux comme clés pour v-for. Utilisez des chaines de caractères ou des nombres à la place.

Pour une utilisation détaillée de l’attribut key, consultez la documentation d’API key.

## Détection de changement dans un tableau

### Méthodes de mutation

Vue surcharge les méthodes de mutation d’un tableau observé afin qu’elles déclenchent également des mises à jour de la vue. Les méthodes encapsulées sont :

* push()
* pop()
* shift()
* unshift()
* splice()
* sort()
* reverse()

### Remplacer un tableau

Les méthodes de mutation, comme leur nom le suggère, modifient le tableau d’origine sur lequel elles sont appelées. En comparaison, il y a aussi des méthodes non-mutatives comme par ex. filter(), concat() et slice(), qui ne changent pas le tableau original mais retourne toujours un nouveau tableau. Quand vous travaillez avec des méthodes non-mutatives, vous pouvez juste remplacer l’ancien tableau par le nouveau :

```javascript
example1.items = example1.items.filter(function (item) {
  return item.message.match(/Foo/)
})
```

Vous pouvez penser que cela va forcer Vue à jeter le DOM existant et à faire de nouveau le rendu de la liste entière ? Par chance, cela n’est pas le cas. Vue implémente plusieurs fonctions heuristiques intelligentes pour maximiser la réutilisation du DOM existant ; ainsi remplacer un tableau par un autre contenant des objets différents à certains index est une opération très performante.

### Limitations

En raison des limitations de JavaScript, il y a des types de changements que Vue ne peut pas détecter avec les tableaux et les objets. Ils sont abordés dans la section réactivité.

## Affichage de résultats filtrés/triés

Parfois nous voulons afficher une version filtrée ou triée d’un tableau sans pour autant modifier ou réassigner les données d’origine. Dans ce cas, vous pouvez créer une propriété calculée qui retourne le tableau filtré ou trié.

```javascript
<li v-for="n in evenNumbers">{{ n }}</li>

data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
computed: {
  evenNumbers: function () {
    return this.numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
```

Dans les situations où les propriétés calculées ne sont pas utilisables (par ex. à l’intérieur d’une boucle v-for imbriquée), vous pouvez juste utiliser une méthode :

```javascript
<ul v-for="set in sets">
  <li v-for="n in even(set)">{{ n }}</li>
</ul>

data: {
  sets: [[ 1, 2, 3, 4, 5 ], [6, 7, 8, 9, 10]]
},
methods: {
  even: function (numbers) {
    return numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
```

## v-for et plage de valeurs