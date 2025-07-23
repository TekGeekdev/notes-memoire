# Les fonctions de rappel (*callback functions*) en JavaScript

## 🧠 1. Qu’est-ce qu’une fonction callback ?

Imagine que tu commandes une pizza.

Tu dis au resto :

> "Quand la pizza est prête, **appelle-moi**."

➡️ **Tu laisses ton numéro → c’est ça une callback.**  
Tu ne dis pas "fais ceci maintenant", tu dis "quand tu auras fini, fais ceci".



## ✅ Une fonction callback, c’est :

- Une **fonction qu’on donne à une autre fonction**.

- Elle ne s’exécute **pas tout de suite**, mais **plus tard**, **quand l’autre fonction décide**.

- C’est souvent utilisé pour dire :  
  **"Quand tu auras fini, exécute ça."**



**👀 Exemple simple**

```js
function direBonjour(callback) {
  console.log("Bonjour !");
  callback(); // ici, on appelle la fonction reçue
}

function direCommentCaVa() {
  console.log("Comment ça va ?");
}

direBonjour(direCommentCaVa);
```

🧾 Résultat :

```js
Bonjour !
Comment ça va ?
```

Ici, on **donne** `direCommentCaVa` à `direBonjour`,  
et `direBonjour` **l’exécute plus tard**.



**Exemple dans la vie réelle : liste de courses**

```js
const fruits = ["pomme", "banane", "kiwi"];

fruits.forEach(function(fruit) {
  console.log("J’achète : " + fruit);
});
```

🧾 Résultat :

```js
J’achète : pomme  
J’achète : banane  
J’achète : kiwi
```

➡️ Ici, `forEach` utilise **un callback** pour **faire quelque chose avec chaque fruit**.



**⏱️ Exemple avec un délai (timer)**

```js
setTimeout(function() {
  console.log("3 secondes sont passées !");
}, 3000); // 3000 = 3 secondes
```

➡️ `setTimeout` **attend 3 secondes**, puis appelle ton **callback**.



## 🧠 Pourquoi utiliser des callbacks ?

- Pour **attendre que quelque chose se termine** (comme un timer)

- Pour **réagir à un événement** (clic, chargement, etc.)

- Pour **réutiliser du code** en changeant ce qui doit être fait



## ⚠️ Attention : bien passer **le nom de la fonction**, pas son **résultat**

```js
// ❌ Mauvais : ici on appelle la fonction tout de suite
direBonjour(direCommentCaVa());

// ✅ Bon : on passe juste le nom, sans les parenthèses
direBonjour(direCommentCaVa);
```

**Version raccourcie (fonction fléchée)**

```js
fruits.forEach(fruit => console.log("J’achète : " + fruit));
```



## 🧺 Résumé visuel

| Terme              | C’est quoi ?                           | Exemple                    |
| ------------------ | -------------------------------------- | -------------------------- |
| Callback           | Fonction donnée à une autre            | `traitement(maFonction)`   |
| Utilisation        | Après un clic, une attente, une action | `setTimeout`, `forEach`    |
| Ne pas faire       | `callback()` → s’exécute tout de suite | Éviter dans les paramètres |
| Raccourci possible | Fonction fléchée                       | `x => x + 1`               |



## 📚 Liens utiles

- [Callback function - Glossary | MDN](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function)

- [Fonctions et portée des fonctions - JavaScript | MDN](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Functions)

- [setTimeout() - Les API Web | MDN](https://developer.mozilla.org/fr/docs/Web/API/Window/setTimeout)

- [MDN - Array.prototype.forEach()](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)


