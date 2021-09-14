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