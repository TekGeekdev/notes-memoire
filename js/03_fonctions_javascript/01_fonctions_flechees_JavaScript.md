# Les fonctions fléchées (`=>`) en JavaScript

## 🧠 1. Qu’est-ce qu’une fonction fléchée ?

Les **fonctions fléchées** sont une **syntaxe plus courte** pour écrire des fonctions anonymes en JavaScript.  
Introduites avec **ES6 (2015)**, elles sont très utiles dans les fonctions de rappel (*callbacks*), les expressions simples, et la programmation fonctionnelle.



## 🆚 2. Différence avec les fonctions classiques

| Caractéristique        | Fonction classique        | Fonction fléchée           |
| ---------------------- | ------------------------- | -------------------------- |
| Syntaxe                | `function nom() {}`       | `const nom = () => {}`     |
| `this`                 | Dynamique (selon l'appel) | Lexical (hérité du parent) |
| Peut être constructeur | ✅ Oui                     | ❌ Non                      |
| Plus concise           | ❌                         | ✅                          |
| `arguments` disponible | ✅                         | ❌                          |



## ✍️ 3. Syntaxe de base

```js
const saluer = () => {
  console.log("Bonjour !");
};

saluer(); // Bonjour !
```



## ⚡ 4. Syntaxe raccourcie (return implicite)

Si ta fonction ne fait **qu’un `return`**, tu peux **enlever les `{}` et le mot `return`** :

```js
const doubler = x => x * 2;

console.log(doubler(5)); // 10
```

🔸 Ici :

- Pas de `function`

- Pas de `{}` ni de `return`

- Et même les parenthèses sont **optionnelles** si un seul paramètre



## 🔥 5. Exemples variés

**A. Sans paramètre :**

```js
const direBonjour = () => "Salut !";
```

**B. Avec plusieurs paramètres :**

```js
const addition = (a, b) => a + b;
```

**C. Avec un bloc de code :**

```js
const direBonsoir = nom => {
  console.log("Bonsoir " + nom);
};
```

**D. Dans une méthode `.map()` :**

```js
const nombres = [1, 2, 3];
const carrés = nombres.map(n => n * n); // [1, 4, 9]
```



## 📌 6. Le comportement de `this`

Les **fonctions fléchées n’ont pas leur propre `this`**, elles héritent du `this` **de leur environnement parent**.

**Exemple :**

```js
function Compteur() {
  this.valeur = 0;

  setInterval(() => {
    this.valeur++;
    console.log(this.valeur);
  }, 1000);
}
```

🔹 Ici, `this.valeur++` fonctionne **grâce à la fonction fléchée**.  
Si on avait utilisé une `function()` normale, le `this` aurait été `undefined` ou référé au contexte global.



## ⚠️ 7. Quand NE PAS utiliser une fonction fléchée

Tu ne dois **pas** utiliser une fonction fléchée si :

- Tu as besoin de ton **propre `this`** (par ex. dans une méthode de classe)

- Tu as besoin d'accéder à `arguments`

- Tu veux créer une fonction constructeur (avec `new`)



## ✅ 8. Résumé

| ✅ Avantages                         | ⚠️ Limitations                              |
| ----------------------------------- | ------------------------------------------- |
| Syntaxe concise                     | Pas de `this`, `arguments`, `super` propres |
| Lexical `this` pratique             | Pas utilisable comme constructeur           |
| Idéal pour callbacks / map / filter | Pas adaptée aux méthodes d'objet            |



## 📚 Liens utiles

- [Fonctions fléchées - JavaScript | MDN](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

- [function - JavaScript | MDN](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Statements/function)

- [opérateur this - JavaScript | MDN](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Operators/this)


