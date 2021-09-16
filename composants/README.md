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

Les composants peuvent être réutilisés autant de fois que souhaité :

```javascript
<div id="components-demo">
  <button-counter></button-counter>
  <button-counter></button-counter>
  <button-counter></button-counter>
</div>
```

Notez que lors du clic sur les boutons, chacun d’entre eux maintient son propre compteur séparé des autres. C’est parce que chaque fois que vous utilisez un composant, une nouvelle instance est créée.

### data doit être une fonction

Quand vous définissez le composant < button-counter >, vous devez faire attention que data ne soit pas directement fourni en tant qu’objet, comme ceci :

```javascript
data: {
  count: 0
}
```

À la place, la propriété du composant data doit être une fonction, afin que chaque instance puisse conserver une copie indépendante de l’objet retourné :

```javascript
data: function () {
  return {
    count: 0
  }
}
```

Si Vue n’avait pas cette règle, cliquer sur un bouton affecterait les données de toutes les autres instances.

## Organisation des composants

Il est commun pour une application d’être organisée en un arbre de composants imbriqués :

Schéma : https://fr.vuejs.org/v2/guide/components.html

Par exemple, vous pouvez avoir des composants pour l’entête, la barre latérale, la zone de contenu ; chacun contenant lui aussi d’autres composants pour la navigation, les liens, les billets de blog, etc.

Pour utiliser ces composants dans des templates, ils doivent être enregistrés pour que Vue les connaisse. Il y a deux types d’enregistrement de composant : global et local. Jusqu’ici, nous avons uniquement enregistré des composants globalement en utilisant Vue.component.

> C’est tout ce que vous avez besoin de savoir à propos de l’enregistrement pour le moment, mais une fois que vous aurez fini de lire cette page et que vous vous sentirez à l’aise avec son contenu, nous vous recommandons de revenir pour lire le guide complet à propos de l’Enregistrement de composant.

## Passer des données aux composants enfants avec les props

Plus tôt, nous avons mentionné la création d’un composant pour des billets de blog. Le problème est que ce composant ne sera utile que si l’on peut lui passer des données, comme le titre ou le contenu pour un billet spécifique à afficher. C’est ici que les props interviennent.

Les props sont des attributs personnalisables que vous pouvez enregistrer dans un composant. Quand une valeur est passée à un attribut prop, elle devient une propriété de l’instance du composant. Pour passer un titre à notre billet de blog, nous devons l’inclure dans une liste de props que ce composant accepte, en utilisant l’option props :

```javascript
Vue.component('blog-post', {
  props: ['title'],
  template: '<h3>{{ title }}</h3>'
})
```

Un composant peut avoir autant de props que vous le souhaitez et par défaut, n’importe quelle valeur peut être passée à une prop. Dans le template ci-dessus, vous devriez voir cette valeur dans l’instance du composant, comme pour data.

Une fois une prop enregistrée, vous pouvez lui passer des données en tant qu’attribut personnalisé comme ceci :

```javascript
<blog-post title="Mon initiation avec Vue"></blog-post>
<blog-post title="Blogger avec Vue"></blog-post>
<blog-post title="Pourquoi Vue est tellement cool"></blog-post>
```

Résultat : https://fr.vuejs.org/v2/guide/components.html

Dans une application typique, cependant, vous préfèreriez avoir un tableau de billets dans data :

```javascript
new Vue({
  el: '#blog-post-demo',
  data: {
    posts: [
      { id: 1, title: 'Mon initiation avec Vue' },
      { id: 2, title: 'Blogger avec Vue' },
      { id: 3, title: 'Pourquoi Vue est tellement cool' }
    ]
  }
})
```

Maintenant, faisons le rendu d’un composant pour chacun :

```javascript
<blog-post
  v-for="post in posts"
  v-bind:key="post.id"
  v-bind:title="post.title"
></blog-post>
```

Vous voyez au-dessus que nous pouvons utiliser v-bind pour dynamiquement passer des props. Cela est particulièrement utile quand vous ne connaissez pas exactement le contenu dont vous êtes en train de faire le rendu à l’avance, comme dans le cas de récupération de billets depuis une API.

C’est tout ce que vous avez besoin de savoir à propos des props pour le moment, mais une fois que vous aurez fini de lire cette page et que vous vous sentirez à l’aise avec son contenu, nous vous recommandons de revenir pour lire le guide complet à propos des props.

## Élément racine unique

Quand nous réalisons un composant < blog-post >, votre template va éventuellement contenir plus que juste le titre :

```javascript
<h3>{{ title }}</h3>
```

Vous allez au moins vouloir inclure le contenu du billet :

```javascript
<h3>{{ title }}</h3>
<div v-html="content"></div>
```

Si vous essayez cela dans votre template cependant, Vue va afficher une erreur, expliquant que tout composant doit avoir un unique élément racine. Vous pouvez corriger cette erreur en imbriquant le template dans un élément parent comme :

```javascript
<div class="blog-post">
  <h3>{{ title }}</h3>
  <div v-html="content"></div>
</div>
```

À mesure que nos composants grandissent, il ne sera plus question uniquement d’un titre et d’un contenu pour le billet, mais également de la date de publication, des commentaires et bien plus. Définir une prop indépendamment pour chaque information pourrait devenir gênant :

```javascript
<blog-post
  v-for="post in posts"
  v-bind:key="post.id"
  v-bind:title="post.title"
  v-bind:content="post.content"
  v-bind:publishedAt="post.publishedAt"
  v-bind:comments="post.comments"
></blog-post>
```

Le temps sera alors venu de refactoriser le composant <blog-post> pour accepter une propriété post unique à la place :

```javascript
<blog-post
  v-for="post in posts"
  v-bind:key="post.id"
  v-bind:post="post"
></blog-post>

Vue.component('blog-post', {
  props: ['post'],
  template: `
    <div class="blog-post">
      <h3>{{ post.title }}</h3>
      <div v-html="post.content"></div>
    </div>
  `
})
```

> L’exemple ci-dessus et plusieurs exemples par la suite utilisent une chaîne de caractères JavaScript appelée modèles de libellés (« template string ») permettant des templates multilignes plus lisibles. Ceux-ci ne sont pas supportés dans Internet Explorer (IE), aussi, si vous souhaitez supporter IE sans utiliser de transpilateur (p. ex. Babel ou TypeScript), ajoutez un caractère d’échappement à chaque nouvelle ligne à la place.

Maintenant, chaque fois qu’une nouvelle propriété sera ajoutée à l’objet post, elle sera automatiquement disponible dans < blog-post >.

## Envoyer des messages aux parents avec les évènements

Lors de notre développement du composant <blog-post>, plusieurs fonctionnalités vont demander de communiquer des informations au parent. Par exemple, nous pourrions décider d’inclure une fonctionnalité d’accessibilité pour élargir le texte du billet de blog, alors que le reste de la page resterait dans sa taille par défaut :

Dans le parent, nous pouvons supporter cette fonctionnalité en ajoutant une propriété de donnée postFontSize :

```javascript
new Vue({
  el: '#blog-posts-events-demo',
  data: {
    posts: [/* ... */],
    postFontSize: 1
  }
})
```

Qui pourrait être utilisé dans un template pour contrôler la taille de la police de tous les billets de blog :

```javascript
<div id="blog-posts-events-demo">
  <div :style="{ fontSize: postFontSize + 'em' }">
    <blog-post
      v-for="post in posts"
      v-bind:key="post.id"
      v-bind:post="post"
    ></blog-post>
  </div>
</div>
```

Maintenant, ajoutons un bouton pour élargir le texte juste avant le contenu de chaque billet :

```javascript
Vue.component('blog-post', {
  props: ['post'],
  template: `
    <div class="blog-post">
      <h3>{{ post.title }}</h3>
      <button>
        Enlarge text
      </button>
      <div v-html="post.content"></div>
    </div>
  `
})
```

Le problème est que le bouton ne fait rien du tout.

Quand nous cliquons sur le bouton, nous avons besoin de communiquer au parent qu’il devrait élargir le texte de tous les billets. Heureusement, l’instance de Vue fournit un système d’évènements personnalisables pour résoudre ce problème. Le parent peut choisir d’écouter n’importe quel évènement de l’instance du composant enfant avec v-on, juste comme il le ferrait avec un évènement natif du DOM :

```javascript
<blog-post
  ...
  v-on:enlarge-text="postFontSize += 0.1"
></blog-post>
```

Puis le composant enfant peut émettre un évènement lui-même en appelant la méthode préconçue $emit, en lui passant le nom de l’évènement :

```javascript
<button v-on:click="$emit('enlarge-text')">
  Élargir le texte
</button>
```

Grace a l’écouteur v-on:enlarge-text="postFontSize += 0.1", le parent va recevoir l’évènement et mettre à jour la valeur de postFontSize.

### Émettre une valeur avec un évènement

Il est parfois utile d’émettre une valeur spécifique avec un évènement. Par exemple, nous pourrions vouloir que le composant < blog-post > soit en charge de comment élargir le texte. Dans ce cas, nous pouvons utiliser $emit en second paramètre pour fournir cette valeur :

```javascript
<button v-on:click="$emit('enlarge-text', 0.1)">
  Élargir le texte
</button>
```

Puis quand nous écoutons l’évènement dans le parent, nous pouvons accéder à la valeur de l’évènement émise avec $event :

```javascript
<blog-post
  ...
  v-on:enlarge-text="postFontSize += $event"
></blog-post>
```

Ou, si le gestionnaire d’évènement est une méthode :

```javascript
<blog-post
  ...
  v-on:enlarge-text="onEnlargeText"
></blog-post>
```

Puis la valeur sera fournie en tant que premier argument de cette méthode :

```javascript
methods: {
  onEnlargeText: function (enlargeAmount) {
    this.postFontSize += enlargeAmount
  }
}
```

### Utiliser v-model sur les composants

Les évènements personnalisés peuvent aussi être utilisés pour créer des champs qui fonctionnent avec v-model. Rappelez-vous cela :

```javascript
<input v-model="searchText">
```

réalise la même chose que :

```javascript
<input
  v-bind:value="searchText"
  v-on:input="searchText = $event.target.value"
>
```

Quand il est utilisé sur un composant, v-model fait plutôt cela :

```javascript
<custom-input
  v-bind:value="searchText"
  v-on:input="searchText = $event"
></custom-input>
```

Pour que cela puisse fonctionner, la balise < input > à l’intérieur du composant doit :
* Lier l’attribut value à la prop value
* Et sur l’input, émettre son propre évènement personnalisé input avec la nouvelle valeur

Voici un exemple en action :

```javascript
Vue.component('custom-input', {
  props: ['value'],
  template: `
    <input
      v-bind:value="value"
      v-on:input="$emit('input', $event.target.value)"
    >
  `
})
```

Maintenant v-model fonctionnera parfaitement avec le composant :

```javascript
<custom-input v-model="searchText"></custom-input>
```

C’est tout ce que vous avez besoin de savoir à propos des évènements pour le moment, mais une fois que vous aurez fini de lire cette page et que vous vous sentirez à l’aise avec son contenu, nous vous recommandons de revenir pour lire le guide complet à propos des évènements personnalisés.

## Distribution de contenu avec les slots

Exactement comme les éléments HTML, il est souvent utile de passer du contenu à un composant comme ceci :

```javascript
<alert-box>
  Quelque chose s'est mal passé.
</alert-box>
```

Heureusement, cette tâche est vraiment simple avec l’élément personnalisé < slot > de Vue :

```javascript
Vue.component('alert-box', {
  template: `
    <div class="demo-alert-box">
      <strong>Erreur !</strong>
      <slot></slot>
    </div>
  `
})
```

Comme vous pouvez le constater plus haut, nous avons seulement ajouté un slot là où nous souhaitions faire atterrir le contenu - et c’est tout. C’est fait !

C’est tout ce que vous avez besoin de savoir à propos des slots pour le moment, mais une fois que vous aurez fini de lire cette page et que vous vous sentirez à l’aise avec son contenu, nous vous recommandons de revenir plus tard pour lire le guide complet à propos des slots.

## Composants dynamiques

Parfois, il est utile de dynamiquement interchanger des composants, comme dans une interface à onglet :

Exemple : https://fr.vuejs.org/v2/guide/components.html

Ce qu’il y a ci-dessus est rendu possible grâce à l’élément < component > de Vue avec l’attribut spécial is :

```javascript
<!-- Component changes when currentTabComponent changes -->
<component v-bind:is="currentTabComponent"></component>
```

Dans l’exemple ci-dessus, currentTabComponent peut contenir soit :

* le nom du composant enregistré, ou
* un objet d’option de composant

Regardez ce fiddle pour expérimenter cela avec un code complet, ou cette version pour un exemple lié à un objet d’option de composant plutôt qu’à un nom enregistré.

Gardez à l’esprit que ces attributs peuvent être utilisés sur des éléments HTML standard, cependant ils seront traités comme des composants, ce qui signifie que tous les attributs seront liés en tant qu’attribut du DOM. Pour que diverses propriétés fonctionnent comme vous le souhaitez, comme pour value, vous allez devoir les liés en utilisant le modificateur .prop.

C’est tout ce que vous avez besoin de savoir à propos des composants dynamiques pour le moment, mais une fois que vous aurez fini de lire cette page et que vous vous sentirez à l’aise avec son contenu, nous vous recommandons de revenir pour lire le guide complet à propos des Composants dynamiques et asynchrones.

## Cas particuliers de l’analyse des templates de DOM

Plusieurs éléments HTML, comme < ul>, < ol>, < table> et < select> ont des restrictions en ce qui concerne les éléments qui peuvent apparaitre à l’intérieur d’eux. D’autres éléments quant à eux, tel que < li>, < tr>, ou < option> peuvent uniquement être placés à l’intérieur de certains éléments parents uniquement.

Cela mène à des problèmes quand vous utilisez des composants avec des éléments qui ont ces restrictions. Par exemple :

```javascript
<table>
  <blog-post-row></blog-post-row>
</table>
```

Le composant personnalisé < blog-post-row> sera considéré comme un contenu invalide, causant des erreurs dans les rendus éventuels en sortie. Heureusement, l’attribut spécial is offre un moyen de contournement :

```javascript
<table>
  <tr is="blog-post-row"></tr>
</table>
```

Il doit être noté que cette limitation n’affecte pas les templates sous forme de chaine de caractères provenant d’une des sources suivantes :

* Un template de chaine de caractères (par ex. template: '...'),
* Les composants monofichier (.vue)
* < script type="text/x-template">

C’est tout ce que vous avez besoin de savoir à propos des cas particuliers pour le moment. Vous voilà arrivé à la fin de l’Essentiel de Vue. Félicitations ! Il reste encore beaucoup à apprendre, mais d’abord, nous vous recommandons de faire une pause pour jouer avec Vue par vous-même et construire quelque chose d’amusant.

Une fois que vous vous sentirez à l’aise avec les connaissances que vous venez fraichement d’acquérir, nous vous recommandons de revenir pour lire le guide complet à propos des Composants dynamiques et asynchrones ainsi que les autres pages de la partie Composants en détails de la barre de navigation latérale.