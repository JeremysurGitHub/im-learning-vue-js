# Liaisons sur les champs de formulaire

## Sommaire

## Usage basique

Vous pouvez utiliser la directive v-model pour créer une liaison de données bidirectionnelle sur les champs de formulaire (input, select ou textarea). Elle choisira automatiquement la bonne manière de mettre à jour l’élément en fonction du type de champ. Bien qu’un peu magique, v-model est essentiellement du sucre syntaxique pour mettre à jour les données lors des évènements de saisie utilisateur sur les champs, ainsi que quelques traitements spéciaux pour certains cas particuliers.

> v-model ne prend pas en compte la valeur initiale des attributs value, checked ou selected fournis par un champ. Elle traitera toujours les données de l’instance de Vue comme la source de vérité. Vous devez déclarer la valeur initiale dans votre JavaScript, dans l’option data de votre composant.

v-model utilise en interne différentes propriétés et émetteurs d’évènement pour différents éléments de saisie :

* Les éléments text et textarea utilisent la propriété value et évènement input;
* Les éléments checkboxes et radiobuttons utilisent la propriété checked et l’évènement change;
* Les éléments select utilisent value comme une prop et change comme un évènement.

> Pour les langues qui requièrent une méthode de saisie (IME) (chinois, japonais, coréen, etc.), vous remarquerez que v-model ne sera pas mise à jour durant l’exécution de la méthode de saisie. Si vous souhaitez également prendre en compte ces mises à jour, utilisez plutôt l’évènement input.

### Texte

```javascript
<input v-model="message" placeholder="modifiez-moi">
<p>Le message est : {{ message }}</p>
```

### Texte multiligne

```javascript
<span>Le message multiligne est :</span>
<p style="white-space: pre-line;">{{ message }}</p>
<br>
<textarea v-model="message" placeholder="ajoutez plusieurs lignes"></textarea>
```

> L'interpolation sur les zones de texte (<textarea>{{text}}</textarea>) ne fonctionnera pas. Utilisez v-model à la place.

### Checkbox

Checkbox seule, valeur booléenne :

```javascript
<input type="checkbox" id="checkbox" v-model="checked">
<label for="checkbox">{{ checked }}</label>
```

Checkboxes multiples, liées au même tableau (Array) :

```javascript
<input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
<label for="jack">Jack</label>
<input type="checkbox" id="john" value="John" v-model="checkedNames">
<label for="john">John</label>
<input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
<label for="mike">Mike</label>
<br>
<span>Noms cochés : {{ checkedNames }}</span>

new Vue({
  el: '...',
  data: {
    checkedNames: []
  }
})
```

### Radio

```javascript
<input type="radio" id="one" value="Un" v-model="picked">
<label for="one">Un</label>
<br>
<input type="radio" id="two" value="Deux" v-model="picked">
<label for="two">Deux</label>
<br>
<span>Choisi : {{ picked }}</span>
```

### Select

Select à choix unique :

```javascript
<select v-model="selected">
  <option disabled value="">Choisissez</option>
  <option>A</option>
  <option>B</option>
  <option>C</option>
</select>
<span>Sélectionné : {{ selected }}</span>

new Vue({
  el: '...',
  data: {
    selected: ''
  }
})
```

> Si la valeur initiale de votre expression dans v-model ne correspond à aucune des options, l’élément <select> va faire le rendu dans un état « non sélectionné ». Sur iOS cela va conduire l’utilisateur à ne pas pouvoir sélectionner le premier élément car aucun évènement change n’est déclenché dans ce cas. Il est cependant recommandé de fournir une option désactivée avec une valeur vide comme dans l’exemple ci-dessus.

Select à choix multiples (lié à un tableau) :

```javascript
<select v-model="selected" multiple>
  <option>A</option>
  <option>B</option>
  <option>C</option>
</select>
<br>
<span>Sélectionné(s) : {{ selected }}</span>
```

Options dynamiques générées avec v-for :

```javascript
<select v-model="selected">
  <option v-for="option in options" v-bind:value="option.value">
    {{ option.text }}
  </option>
</select>
<span>Sélectionné : {{ selected }}</span>

new Vue({
  el: '...',
  data: {
    selected: 'A',
    options: [
      { text: 'Un', value: 'A' },
      { text: 'Deux', value: 'B' },
      { text: 'Trois', value: 'C' }
    ]
  }
})
```

## Liaisons des attributs value

