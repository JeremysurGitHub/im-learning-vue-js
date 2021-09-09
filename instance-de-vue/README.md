# Instance de Vue

## Sommaire

* Créer une instance de Vue : déclaration et objet d’options.

* Données et méthodes : propriétés de l'objet data, système réactif, rendu de la vue, Object.freeze(), méthodes et propriétés avec le $.

* Hooks de cycle de vie d’une instance : série d’étapes, pas de fonctions fléchées.

* Diagramme du cycle de vie : voir la doc.

## Créer une instance de Vue

Chaque application Vue est initialisée en créant une nouvelle instance de Vue avec la fonction Vue.

```javascript
var vm = new Vue({
  // options
})
```

Par convention, nous utilisons souvent la variable vm (abréviation pour ViewModel) pour faire référence à nos instances de Vue.

Quand vous créez une instance de Vue, vous devez passer un objet d’options.

Une application Vue consiste en une instance racine de Vue créée avec new Vue et optionnellement organisée en un arbre de composants imbriqués et réutilisables.

Nous parlerons du système de composant en détail plus loin. Pour le moment, sachez simplement que tous les composants de Vue sont également des instances de Vue et qu’ils acceptent donc le même objet d’option (à l’exception de quelques options spécifiques à l’instance racine).

## Données et méthodes

Quand une instance de Vue est créée, cela ajoute toutes les propriétés trouvées dans son objet data au système réactif de Vue. Quand une valeur de ces propriétés change, la vue va « réagir », se mettant à jour pour concorder avec les nouvelles valeurs.

Quand ces données changent, le rendu de la vue est refait. Il est à noter que les propriétés dans data sont réactives SI elles existaient quand l’instance a été créée. 

Si vous savez que vous allez avoir besoin d’une propriété plus tard qui n’a pas de valeur dès le début, vous avez juste besoin de la créer avec n’importe quelle valeur initiale.

La seule exception à cela est l’utilisation de Object.freeze(), qui empêche les propriétés existantes d’être changées, ce qui implique que le système de réactivité ne peut pas traquer les changements.

En plus des propriétés de données, les instances de Vue exposent de nombreuses méthodes et propriétés utiles. Ces propriétés et méthodes sont préfixées par $ pour les différencier des propriétés proxifiées de data.

```javascript
// $watch est une méthode de l'instance
vm.$watch('a', function (newVal, oldVal) {
  // cette fonction de rappel sera appelée quand `vm.a` changera
})
```

Consultez l’API pour une liste complète des propriétés et méthodes d’une instance.

## Hooks de cycle de vie d’une instance

Chaque instance de vue traverse une série d’étapes d’initialisation au moment de sa création - par exemple, elle doit mettre en place l’observation des données, compiler le template, monter l’instance sur le DOM et mettre à jour le DOM quand les données changent. En cours de route, elle va aussi invoquer des hooks de cycle de vie, qui nous donnent l’opportunité d’exécuter une logique personnalisée à chaque niveau.

Par exemple, le hook created est appelé une fois l’instance créée.
Il y a aussi d’autres hooks qui seront appelés à différentes étapes du cycle de vie d’une instance, par exemple mounted, updated et destroyed. Tous ces hooks de cycle de vie sont appelés avec leur this pointant sur l’instance de la vue qui les invoque.

> N’utilisez pas les fonctions fléchées sur une propriété ou fonction de rappel d’une instance (par exemple created: () => console.log(this.a) ou vm.$watch('a', newVal => this.myMethod())). Comme les fonctions fléchées sont liées au contexte parent, this ne sera pas l’instance de Vue comme vous pourriez vous y attendre, et vous rencontrerez alors des erreurs comme Uncaught TypeError: Cannot read property of undefined ou Uncaught TypeError: this.myMethod is not a function.

## Diagramme du cycle de vie

https://fr.vuejs.org/v2/guide/instance.html