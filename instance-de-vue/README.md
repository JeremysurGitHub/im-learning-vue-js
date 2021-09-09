# Instance de Vue

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