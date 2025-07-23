# 📌 Les Événements en JavaScript

## 🧠 Qu’est-ce qu’un événement ?

En JavaScript, un **événement** est une **interaction ou un changement** dans la page qui peut être **détecté** par le navigateur (et donc par ton script).

> 📣 **Exemples d’événements :**

- L’utilisateur clique sur un bouton

- Il tape du texte dans un champ

- Il survole un élément avec la souris

- La page finit de se charger

- Une touche du clavier est pressée

Chaque événement peut **déclencher une fonction** qu'on appelle **gestionnaire d'événement** (*event handler=gestionnaire d'événements*).

## ⚙️ Comment ça marche ?

### Étapes :

1. Tu **cibles un élément** HTML avec JavaScript

2. Tu **écoutes un événement** spécifique sur cet élément

3. Tu **exécutes une fonction** quand l’événement se produit

## 🧪 Syntaxe de base : `addEventListener`

```js
const bouton = document.querySelector("button");

function handleClick() {
  alert("Bouton cliqué !");
}

bouton.addEventListener("click", handleClick);
```

## 🧠 Subtilité importante : la suppression d’un écouteur

Tu ne peux **retirer un gestionnaire d’événement** (`removeEventListener`) **que si la fonction passée est une référence nommée** (et non une fonction anonyme ou fléchée inline).

#### ❌ Mauvais exemple (non retirable) :

```js
button.addEventListener("click", () => {
  console.log("clic");
});

// Ne fonctionnera pas :
button.removeEventListener("click", () => {
  console.log("clic");
});
```

#### ✅ Bon exemple (retirable) :

```js
function handleClick() {
  console.log("clic");
}

button.addEventListener("click", handleClick);
button.removeEventListener("click", handleClick);
```

## 

## 🎯 Principaux types d’événements

| Catégorie      | Événement                  | Description                           |
| -------------- | -------------------------- | ------------------------------------- |
| Souris         | `click`                    | Clic simple                           |
|                | `dblclick`                 | Double clic                           |
|                | `mouseenter`, `mouseleave` | Entrée/sortie de la souris            |
|                | `mousedown`, `mouseup`     | Clic enfoncé / relâché                |
| Clavier        | `keydown`                  | Touche pressée                        |
|                | `keyup`                    | Touche relâchée                       |
|                | `keypress` (obsolète)      | Touche tapée (éviter)                 |
| Formulaire     | `submit`                   | Envoi d’un formulaire                 |
|                | `change`                   | Changement de valeur (select, input…) |
|                | `input`                    | À chaque frappe dans un champ texte   |
| Page / fenêtre | `load`                     | Page ou ressource chargée             |
|                | `DOMContentLoaded`         | HTML chargé, sans attendre images     |
|                | `resize`                   | Fenêtre redimensionnée                |
|                | `scroll`                   | Utilisateur fait défiler la page      |

## 🔄 Gérer l’objet `event`

Quand un événement se déclenche, une **fonction de rappel (callback)** reçoit automatiquement un **objet `event`** en argument.

**Exemple :**

```js
document.addEventListener("click", function (e) {
  console.log("Cible cliquée :", e.target);
});
```

**Propriétés utiles :**

- `event.type` → type d’événement

- `event.target` → élément déclencheur

- `event.preventDefault()` → empêche comportement par défaut

- `event.stopPropagation()` → arrête la propagation

**🧪 Exemples concrets**

Empêcher le rechargement d’un formulaire

```js
const form = document.querySelector("form");

form.addEventListener("submit", handleForm);

 function handleForm(e){
  e.preventDefault(); // empêche l’envoi réel
  alert("Formulaire intercepté !");
};
```

Récupérer les touches du clavier

```js
document.addEventListener("keydown", (e) => {
  console.log(`Touche pressée : ${e.key}`);
});

// ou


document.addEventListener("keydown", handlePressKeyboard);

function handlePressKeyboard(e){
    console.log(`Touche pressée : ${e.key}`);
};
```

Supprimer un écouteur d’événement

```js
const bouton = document.querySelector("button");

function handleClick() {
  alert("Action !");
}

bouton.addEventListener("click", handleClick);

// Supprimer l'écouteur :
bouton.removeEventListener("click", handleClick);
```

**Important :** pour pouvoir le retirer, la fonction doit être **nommée** (pas une arrow function inline).

## 🧠 Propagation d’événement : Bubbling & Capture

Quand un événement se déclenche, il peut se **propager** dans l’arborescence du DOM.

- **Capture** : du document vers l’élément ciblé
- **Bubbling (par défaut)** : de l’élément vers le haut du DOM

### Étapes :

1. **Phase de capture** (du haut vers l’élément)

2. **Ciblage de l’élément** (où l’événement a lieu)

3. **Phase de bubbling** (de l’élément vers le haut)

> 🔄 Par défaut, les événements sont capturés en **bubbling**.

**Empêcher la propagation :**

```js
e.stopPropagation();
```

🔍 Exemple :

```html
<div id="parent">
  <button id="child">Clique-moi</button>
</div>
```

```js
const parent = document.getElementById("parent");
const child = document.getElementById("child");

function onParentClick() {
  console.log("Parent cliqué");
}

function onChildClick(e) {
  e.stopPropagation();
  console.log("Bouton cliqué sans propagation");
}

parent.addEventListener("click", onParentClick);
child.addEventListener("click", onChildClick);
```

## 💡 Astuces et bonnes pratiques

✅ Toujours tester si l’élément existe avant d’ajouter un `addEventListener`  
✅ Utiliser `DOMContentLoaded` pour s’assurer que le DOM est prêt  
✅ Préférer `addEventListener` à `onclick` en HTML

```js
document.addEventListener("DOMContentLoaded", () => {
  // DOM prêt, tu peux manipuler les éléments
});
```

## 🧰 Outils de débogage pour les événements

- 🔍 **Inspecteur d’élément** : onglet "Événements"

- 🧪 `console.log(e)` ou `console.dir(e)` pour examiner l’événement

- 📋 `event.target` pour voir ce qui a déclenché l’événement

- 🧠 Afficher les événements liés via DevTools → Écouteurs

## 📝 Résumé

| Élément                 | Description                          |
| ----------------------- | ------------------------------------ |
| `addEventListener()`    | Attacher un événement                |
| `event.target`          | Élément déclencheur                  |
| `preventDefault()`      | Bloque le comportement par défaut    |
| `stopPropagation()`     | Stoppe la propagation                |
| `removeEventListener()` | Supprime un écouteur                 |
| ⚠️ Fonction nommée      | Nécessaire pour supprimer l’écouteur |

## 📚 Ressources utiles

- [📘 MDN Web Docs - addEventListener](https://developer.mozilla.org/fr/docs/Web/API/EventTarget/addEventListener)
- [📘 MDN - Liste des événements](https://developer.mozilla.org/fr/docs/Web/Events)
- [📘 MDN - event](https://developer.mozilla.org/fr/docs/Web/API/Event)

> Pour découvrir tous les événements possibles, consulte : [Liste complète sur MDN](https://developer.mozilla.org/fr/docs/Web/Events)

## 

## Bonus

### 🔁 Callback – Fonction de rappel

Une **fonction callback** est une fonction **passée en argument** à une autre fonction, et **appelée plus tard** lors d'un événement ou d'une opération.

### Exemple avec événement :

```js
function direBonjour() {
  alert("Bonjour !");
}

button.addEventListener("click", direBonjour); 
// `direBonjour` est le callback ici
```

Les callbacks sont utilisées partout : événements, timers, requêtes asynchrones, etc.

### 🔃 Différence : `addEventListener` vs `onclick`

| `addEventListener`                       | `onclick` (attribut ou propriété JS) |
| ---------------------------------------- | ------------------------------------ |
| Peut attacher **plusieurs écouteurs**    | Écrase les écouteurs précédents      |
| Permet le **mode capture** (3e argument) | Non configurable                     |
| Recommandé pour des projets modulaires   | À éviter pour des scripts complexes  |

```js
// addEventListener
el.addEventListener("click", handleClick);

// onclick
el.onclick = handleClick; // limite à un seul
```
