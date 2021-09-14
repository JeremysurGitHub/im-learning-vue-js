# Gestion des évènements

## Sommaire

## Écouter des évènements

Nous pouvons utiliser l’instruction v-on pour écouter les évènements du DOM afin d’exécuter du JavaScript lorsque ces évènements surviennent.

```javascript
<div id="example-1">
  <button v-on:click="counter += 1">Add 1</button>
  <p>Le bouton ci-dessus a été cliqué {{ counter }} fois.</p>
</div>

var example1 = new Vue({
  el: '#example-1',
  data: {
    counter: 0
  }
})
```

Résultat : https://fr.vuejs.org/v2/guide/events.html

## Méthodes des gestionnaires d’évènements

La logique avec beaucoup de gestionnaires d’évènements sera très certainement plus complexe, par conséquent, garder votre JavaScript dans la valeur de l’attribut v-on ne sera tout simplement pas faisable. C’est pourquoi v-on peut aussi accepter le nom d’une méthode que vous voudriez appeler.

```javascript
<div id="example-2">
  <!-- `greet` est le nom de la méthode définie ci-dessous -->
  <button v-on:click="greet">Greet</button>
</div>

var example2 = new Vue({
  el: '#example-2',
  data: {
    name: 'Vue.js'
  },
  // Définissez les méthodes de l'objet
  methods: {
    greet: function (event) {
      // `this` fait référence à l'instance de Vue à l'intérieur de `methods`
      alert('Bonjour ' + this.name + ' !')
      // `event` est l'évènement natif du DOM
      if (event) {
        alert(event.target.tagName)
      }
    }
  }
})

// vous pouvez également invoquer ces méthodes en JavaScript
example2.greet() // -> 'Bonjour Vue.js !
```

## Méthodes appelées dans les valeurs d’attributs

