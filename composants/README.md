# Composants

## Sommaire

## Exemple de base

Voici un exemple de composant Vue :

```javascript
// Définition d'un nouveau composant appelé `button-counter`
Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">Vous m\'avez cliqué {{ count }} fois.</button>'
})
```

Les composants sont des instances de Vue réutilisables avec un nom : dans notre cas < button-counter >. Nous pouvons utiliser ce composant en tant qu’élément personnalisé à l’intérieur d’une instance de Vue racine créée avec new Vue :

```javascript
<div id="components-demo">
  <button-counter></button-counter>
</div>

new Vue({ el: '#components-demo' })
```

Résultat : https://fr.vuejs.org/v2/guide/components.html

Puisque les composants sont des instances de Vue réutilisables, ils acceptent les mêmes options que new Vue comme data, computed, watch, methods, et les hooks du cycle de vie. Les seules exceptions sont quelques options spécifiques à la racine comme el.

## Réutilisation de composants