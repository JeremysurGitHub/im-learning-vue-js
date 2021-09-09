# Syntaxe de template

## Sommaire

* Description :

* Interpolations : 

* Directives : 

* Abréviations : 

## Description

Vue.js utilise une syntaxe de template basée sur le HTML qui vous permet de lier déclarativement le DOM rendu aux données de l’instance sous-jacente de Vue. Tous les templates de Vue.js sont du HTML valide qui peut être interprété par les navigateurs et les interpréteurs HTML conformes aux spécifications.

Sous le capot, Vue compile les templates en des fonctions de rendus de DOM virtuel. Combiné au système de réactivité, Vue est en mesure de déterminer intelligemment le nombre minimal de composants pour lesquels il faut redéclencher le rendu et d’appliquer le nombre minimal de manipulations au DOM quand l’état de l’application change.

Si vous êtes familiers avec les concepts de DOM virtuel et que vous préférez la puissance du JavaScript pur, vous pouvez aussi écrire directement des fonctions de rendu à la place des templates, avec un support facultatif de JSX.

## Interpolations

### Texte

La forme la plus élémentaire de la liaison de données est l’interpolation de texte en utilisant la syntaxe “Mustache” (les doubles accolades)

```javascript
<span>Message: {{ msg }}</span>
```

La balise moustache sera remplacée par la valeur de la propriété msg de l’objet data correspondant. Elle sera également mise à jour à chaque fois que la propriété msg de l’objet data changera.

Vous pouvez également réaliser des interpolations à usage unique qui ne se mettront pas à jour lors de la modification des données en utilisant la directive v-once, mais gardez à l’esprit que cela affectera toutes les liaisons de données présentes sur le même nœud :

```javascript
<span v-once>Ceci ne changera jamais : {{ msg }}</span>
```

### Interprétation du HTML

Les doubles moustaches interprètent la donnée en tant que texte brut, pas en tant que HTML. Pour afficher réellement du HTML, vous aurez besoin d’utiliser la directive v-htmldirective :

```javascript
<span v-html="rawHtml"></span>
```

Le contenu de cette span sera alors remplacée par la valeur de la propriété rawHtml de l'objet data en tant que HTML classique. Les liaisons de données sont ignorées. À noter que vous ne pouvez pas utiliser v-html pour composer des fragments de templates, parce que Vue n’est pas un moteur de template basé sur les chaines de caractères. À la place, les composants sont préférés en tant qu’unité fondamentale pour la réutilisabilité et la composition de l’interface utilisateur.

> Générer dynamiquement du HTML arbitraire sur votre site peut être très dangereux car cela peut mener facilement à des vulnérabilités XSS. Utilisez l’interpolation HTML uniquement sur du contenu de confiance et jamais sur du contenu fourni par l’utilisateur.

### Attributs

Les moustaches ne peuvent pas être utilisées à l’intérieur des attributs HTML. À la place utilisez une directive v-bind :

```javascript
<div v-bind:id="dynamicId"></div>
```

Dans le cas des attributs booléens qui impliquent la présence d’une valeur évaluée à true, v-bind fonctionne un peu différemment. Dans cet exemple :

```javascript
<button v-bind:disabled="isButtonDisabled">Button</button>
```

Si isButtonDisabled a la valeur null, undefined, ou false, l’attribut disabled ne sera pas inclus dans l’élément <button> généré.

### Utilisation des expressions JavaScript



## Directives

## Abréviations

### Interpolations
