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

Au lieu de lier directement la méthode par son nom dans l’attribut, nous pouvons également utiliser des méthodes dans une instruction JavaScript :

```javascript
<div id="example-3">
  <button v-on:click="say('salut')">Dire salut</button>
  <button v-on:click="say('quoi')">Dire quoi</button>
</div>

new Vue({
  el: '#example-3',
  methods: {
    say: function (message) {
      alert(message)
    }
  }
})
```

Parfois nous avons également besoin d’accéder à l’évènement original du DOM depuis l’instruction dans l’attribut. Vous pouvez le passer à une méthode en utilisant la variable spéciale $event :

```javascript
<button v-on:click="warn('Le formulaire ne peut être soumis pour le moment.', $event)">
  Soumettre
</button>

// ...
methods: {
  warn: function (message, event) {
    // maintenant nous avons accès à l'évènement natif
    if (event) {
      event.preventDefault()
    }
    alert(message)
  }
}
```

## Modificateurs d’évènements

C’est un besoin courant que de faire appel à event.preventDefault() ou event.stopPropagation() à l’intérieur d’une déclaration de gestionnaire d’évènements. Bien que nous puissions réaliser ceci aisément à l’intérieur des méthodes, il serait préférable que les méthodes restent purement dédiées à la logique des données au lieu d’avoir à gérer les détails des évènements du DOM.

Pour résoudre ce problème, Vue propose des modificateurs d’évènements pour v-on. Rappelez-vous que les modificateurs sont des suffixes de directives indiqués par un point.

* .stop
* .prevent
* .capture
* .self
* .once
* .passive

```javascript
<!-- la propagation de l'évènement `click` sera stoppée -->
<a v-on:click.stop="doThis"></a>

<!-- l'évènement `submit` ne rechargera plus la page -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- les modificateurs peuvent être chainés -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- seulement le modificateur -->
<form v-on:submit.prevent></form>

<!-- utilise le mode « capture » lorsque l'écouteur d'évènements est ajouté -->
<!-- c.-à-d. qu'un évènement destiné à un élément interne est géré ici avant d'être géré par ses éléments parents -->
<div v-on:click.capture="doThis">...</div>

<!-- seulement déclenché si l'instruction `event.target` est l'élément lui-même -->
<!-- c.-à-d. que cela ne s'applique pas aux éléments enfants -->
<div v-on:click.self="doThat">...</div>
```

> L’ordre a de l’importance quand vous utilisez des modificateurs car le code est généré dans le même ordre. Aussi utiliser v-on:click.prevent.self va empêcher tous les clics alors que v-on:click.self.prevent va uniquement empêcher le clic sur l’élément lui-même.

> Nouveau dans la 2.1.4+

```javascript
<!-- l'évènement « click » sera déclenché au plus une fois -->
<a v-on:click.once="doThis"></a>
```

Contrairement aux autres modificateurs, qui sont exclusifs aux évènements natifs du DOM, le modificateur .once peut également être utilisé pour les évènements des composants. Si vous n’avez pas encore lu la section concernant les composants, ne vous en inquiétez pas pour le moment.

> Nouveau en 2.3.0+

Vue offre également un modificateur .passive correspondant à l’option passive de addEventListener.

```javascript
<!-- le comportement par défaut de l'évènement de défilement se produit immédiatement, -->
<!-- au lieu d'attendre que `onScroll` soit terminé -->
<!-- au cas où il contienne `event.preventDefault()` -->
<div v-on:scroll.passive="onScroll">...</div>
```

Le modificateur .passive est particulièrement pratique pour améliorer les performances sur mobile.

> N’utilisez pas .passive et .prevent ensemble. .passive sera ignoré et votre navigateur va probablement vous montrer un message. Souvenez-vous, .passive communique au navigateur que vous ne voulez pas prévenir le comportement de l’évènement par défaut.

## Modificateurs de code des touches