# ✨ Les fonctions en JavaScript

## 📖 Qu’est-ce qu’une fonction ?

Une **fonction** est un bloc de code qui réalise une tâche spécifique.  
✅ Elle peut recevoir des **paramètres** (entrées).  
✅ Elle peut **retourner une valeur** (sortie).  
✅ Elle peut être appelée plusieurs fois.

## 📝 Définir une fonction

**✅ 1. Déclaration de fonction classique**

```js
function sayHello() {
  console.log("Hello!");
}
```

✅ Appel :

```js
sayHello(); // "Hello!"
```

**✅ 2. Expression de fonction**

Tu peux **stocker une fonction dans une variable** :

```js
const greet = function() {
  console.log("Hi!");
};
```

**✅ 3. Fonction fléchée (*arrow function*)**

Syntaxe concise introduite en ES6 :

```js
const add = (a, b) => {
  return a + b;
};
```

✅ Version courte si un seul return :

```js
const square = x => x * x;
```

**✅ 4. Fonction constructeur (rarement utilisée)**

```js
const sum = new Function("a", "b", "return a + b;");
```

## 📚 Les paramètres

Une fonction peut prendre des paramètres :

```js
function greet(name) {
  console.log(`Hello, ${name}!`);
}

greet("Alice"); // "Hello, Alice!"
```

✅ Les paramètres non passés valent `undefined`.

**Valeurs par défaut**

Depuis ES6, tu peux donner des valeurs par défaut :

```js
function greet(name = "Anonyme") {
  console.log(`Bonjour, ${name}!`);
}

greet(); // "Bonjour, Anonyme!"
```

## 🔄 La valeur de retour

Pour **retourner une valeur**, on utilise `return` :

```js
function add(a, b) {
  return a + b;
}

let result = add(2, 3); // 5
```

Si tu n’écris pas `return`, la fonction retourne `undefined`.

## 🎯 Fonctions anonymes

Une fonction **sans nom** :

```js
setTimeout(function() {
  console.log("Delayed!");
}, 1000);
```

Avec une fonction fléchée :

```js
setTimeout(() => {
  console.log("Arrow function!");
}, 1000);
```

## 🔀 Les fonctions comme valeurs

En JavaScript, les fonctions sont des **objets de première classe** :

- On peut les stocker dans des variables.

- On peut les passer en argument.

- On peut les retourner.

Exemple : une fonction qui retourne une autre fonction

```js
function createMultiplier(multiplier) {
  return function (value) {
    return value * multiplier;
  };
}

const double = createMultiplier(2);
const triple = createMultiplier(3);

console.log(double(5)); // 10
console.log(triple(5)); // 15
```

#### 🧠 Explication :

- `createMultiplier(2)` retourne une fonction qui **multiplie par 2**

- On peut ensuite **utiliser** cette fonction retournée comme une variable

- C’est un exemple classique de **fonction de haut niveau**

## 🧩 Portée (Scope)

Les variables déclarées **dans une fonction** ne sont accessibles que **dans cette fonction** :

```js
function test() {
  let secret = "hidden";
  console.log(secret);
}

test(); // "hidden"
// console.log(secret); // Erreur : secret n'existe pas ici
```



## 🧭 Hoisting

**Définition** :  
Le **hoisting** (en français, *levage* ou *élévation*) est un mécanisme interne de JavaScript qui consiste à **déplacer les déclarations de variables et de fonctions en haut de leur scope (portée)** **avant l’exécution du code**.

---

#### 🎯 En d’autres termes :

Quand le moteur JavaScript lit votre code, il fait d’abord **l’initialisation des déclarations** avant de lancer l’exécution.  
C’est pourquoi certaines choses fonctionnent avant même leur déclaration dans le code.

✅ Les **déclarations de fonction** sont "hoistées" (levées) en haut du scope :

```js
sayHi();

function sayHi() {
  console.log("Hi!");
}
```

⚠️ Mais **les expressions de fonction** ne le sont pas :

```js
// greet(); // Erreur

const greet = function() {
  console.log("Hello!");
};
```



## ✅ Fonctions fléchées vs fonctions classiques

| Fonction classique                    | Fonction fléchée                             |
| ------------------------------------- | -------------------------------------------- |
| `function nom() {}`                   | `const nom = () => {}`                       |
| A son propre `this`                   | Hérite du `this` parent                      |
| Peut être hoistée                     | Pas hoistée                                  |
| Peut être utilisée comme constructeur | Ne peut pas être utilisée comme constructeur |

Exemple de `this`

```js
const obj = {
  value: 42,
  method: function() {
    console.log(this.value);
  },
  arrow: () => {
    console.log(this.value);
  }
};

obj.method(); // 42
obj.arrow(); // undefined (this global)
```



## 🔄 Fonction récursive

Une fonction qui **s’appelle elle-même** :

```js
function countdown(n) {
  if (n <= 0) return;
  console.log(n);
  countdown(n - 1);
}

countdown(3);
// 3
// 2
// 1
```



## 📝 Résumé des syntaxes

✅ **Déclaration classique**

```js
function nom(param) {
  return param;
}
```

✅ **Expression**

```js
const nom = function(param) {
  return param;
};
```

✅ **Fléchée**

```js
const nom = param => param;
```



## ✅ Bonnes pratiques

- Donnez des noms clairs aux fonctions.

- Utilisez `return` explicitement si vous attendez une valeur.

- Utilisez les fléchées si vous ne voulez pas un `this` propre.

- Attention au hoisting !



## 📚 Liens utiles

- [MDN - Functions](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Functions)

- [MDN - Arrow functions](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Functions/Arrow_functions)


