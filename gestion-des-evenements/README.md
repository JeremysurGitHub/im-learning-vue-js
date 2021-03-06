# Gestion des évènements

## Sommaire

* Écouter des évènements : Nous pouvons utiliser l’instruction v-on pour écouter les évènements du DOM afin d’exécuter du JavaScript lorsque ces évènements surviennent.

* Méthodes des gestionnaires d’évènements : La logique avec beaucoup de gestionnaires d’évènements sera très certainement plus complexe, par conséquent, v-on peut aussi accepter le nom d’une méthode que vous voudriez appeler.

* Méthodes appelées dans les valeurs d’attributs : Au lieu de lier directement la méthode par son nom dans l’attribut, nous pouvons également utiliser des méthodes dans une instruction JavaScript. Parfois nous avons également besoin d’accéder à l’évènement original du DOM depuis l’instruction dans l’attribut. Vous pouvez le passer à une méthode en utilisant la variable spéciale $event.

* Modificateurs d’évènements : Il serait préférable que les méthodes restent purement dédiées à la logique des données au lieu d’avoir à gérer les détails des évènements du DOM. Vue propose des modificateurs d’évènements pour v-on. Rappelez-vous que les modificateurs sont des suffixes de directives indiqués par un point, .stop par exemple. L’ordre a de l’importance quand vous utilisez des modificateurs car le code est généré dans le même ordre. Le modificateur .passive est particulièrement pratique pour améliorer les performances sur mobile. N’utilisez pas .passive et .prevent ensemble. .passive sera ignoré et votre navigateur va probablement vous montrer un message. 

* Modificateurs de code des touches : Lorsque nous écoutons les évènements du clavier, nous avons régulièrement besoin de nous assurer du code des touches. Vue permet également d’ajouter un modificateur de touches pour v-on.

* Modificateurs de touches système : Vous pouvez utiliser les modificateurs suivants pour déclencher un évènement du clavier ou de la souris seulement lorsque la touche du modificateur correspondante est pressée.

* Le modificateur .exact permet le contrôle de la combinaison de touches système exacte requise pour déclencher le gestionnaire d’évènements.

* Modificateurs de boutons de la souris : Ces modificateurs n’autorisent la gestion de l’évènement que s’il a été déclenché par un bouton spécifique de la souris.

* Pourquoi des écouteurs dans le HTML ? : 3 avantages.

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

Lorsque nous écoutons les évènements du clavier, nous avons régulièrement besoin de nous assurer du code des touches. Vue permet également d’ajouter un modificateur de touches pour v-on:

```javascript
<!-- faire appel à `vm.submit()` uniquement quand le code de la touche est `Enter` -->
<input v-on:keyup.enter="submit">
```

Vous pouvez également utiliser n’importe quel nom de touche clavier valide fourni par KeyboardEvent.key en tant que modificateur en les écrivant au format kebab-case :

```javascript
<input @keyup.page-down="onPageDown">
```

Dans l’exemple ci-dessus, le gestionnaire va uniquement être appelé si $event.key est égale à ‘PageDown’`.

### Code des touches

> L’utilisation des évènements keyCode est déprécié et pourrait ne plus être supportée dans de futurs navigateurs.

> J'ignore cette partie.

## Modificateurs de touches système

> Nouveau dans la 2.1.0+

Vous pouvez utiliser les modificateurs suivants pour déclencher un évènement du clavier ou de la souris seulement lorsque la touche du modificateur correspondante est pressée :

* .ctrl
* .alt
* .shift
* .meta

> Note: Sur les claviers Macintosh, meta est la touche commande (⌘). Sur Windows, meta est la touche windows (⊞). Sur les claviers Sun Microsystems, meta est symbolisée par un diamant plein (◆). Sur certains claviers, spécifiquement sur les claviers des machines MIT et Lisp et leurs successeurs, comme le clavier « Knight » et « space-cadet », meta est écrit « META ». Sur les claviers Symboliques, meta est étiqueté « META » ou « Meta ».

Par exemple :

```javascript
<!-- Alt + C -->
<input v-on:keyup.alt.67="clear">

<!-- Ctrl + Click -->
<div v-on:click.ctrl="doSomething">Do something</div>
```

> Notez que ces modificateurs de raccourcis sont différents des modificateurs usuels utilisés avec l’évènement keyup, ils doivent être pressés quand l’évènement est émis. En d’autres mots, keyup.ctrl sera déclenché uniquement si vous relâchez une touche pendant que vous maintenez la touche ctrl enfoncée. Rien ne sera déclenché si vous relâchez uniquement la touche Ctrl. Si vous souhaitez un tel comportement, utilisez le keyCode pour ctrl à la place : keyup.17.

### Modificateur .exact

> Nouveau dans la 2.5.0+

Le modificateur .exact permet le contrôle de la combinaison de touches système exacte requise pour déclencher le gestionnaire d’évènements.

```javscript
<!-- ceci va aussi émettre un évènement si les touches Alt et Shift sont pressées -->
<button v-on:click.ctrl="onClick">A</button>

<!-- ceci va émettre un évènement seulement si la touche Ctrl est pressée sans aucune autre touche -->
<button v-on:click.ctrl.exact="onCtrlClick">A</button>

<!-- ceci va émettre un évènement si aucune touche n'est pressée -->
<button v-on:click.exact="onClick">A</button>
```

### Modificateurs de boutons de la souris

> Nouveau dans la 2.2.0+

* .left
* .right
* .middle

Ces modificateurs n’autorisent la gestion de l’évènement que s’il a été déclenché par un bouton spécifique de la souris.

## Pourquoi des écouteurs dans le HTML ?

Vous pourriez être inquiet du fait que l’ensemble de cette approche d’écoute d’évènements viole la bonne vieille règle de la séparation des préoccupations. Rassurez-vous - puisque toutes les fonctions et expressions sont strictement liées au « ViewModel » qui gère la vue courante, cela ne causera aucune difficulté de maintenance. En réalité, il y a plusieurs avantages à utiliser v-on :

1. Il est plus facile de localiser l’implémentation des fonctions gestionnaires dans votre code JS en survolant le code HTML.

1. Comme vous n’avez pas à attacher manuellement les écouteurs dans votre JS, le code du « ViewModel » peut être purement logique et sans DOM. Ceci le rend plus facile à tester.

1. Quand un « ViewModel » est détruit, tous les écouteurs d’évènements sont automatiquement retirés. Vous n’avez pas à vous soucier de le faire vous-même.