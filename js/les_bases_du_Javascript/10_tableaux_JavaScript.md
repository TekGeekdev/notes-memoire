# 🧮 Les Tableaux en JavaScript

## 📖 Qu’est-ce qu’un tableau ?

Un **tableau** (*array*) est une structure de données **ordonnée et indexée**, capable de contenir plusieurs valeurs.

👉 Chaque élément a un **index numérique**, qui commence à **0**.

**Déclaration d’un tableau**

```js
let fruits = ["pomme", "banane", "cerise"];
let nombres = [1, 2, 3, 4, 5];
let vide = [];
```

**Accéder aux éléments**

```js
console.log(fruits[0]); // "pomme"
console.log(fruits[2]); // "cerise"
```

**Accéder à un index inexistant retourne `undefined` :**

```js
console.log(fruits[5]); // undefined
```

**Modifier un élément**

```js
fruits[1] = "kiwi"; // ["pomme", "kiwi", "cerise"]
```

**Taille du tableau**

```js
console.log(fruits.length); // 3
```

**Ajouter / Supprimer**

| Méthode     | Description       | Exemple                    |
| ----------- | ----------------- | -------------------------- |
| `push()`    | Ajoute à la fin   | `fruits.push("mangue")`    |
| `pop()`     | Retire le dernier | `fruits.pop()`             |
| `unshift()` | Ajoute au début   | `fruits.unshift("orange")` |
| `shift()`   | Retire le premier | `fruits.shift()`           |

**Méthodes de manipulation**

| Méthode                  | Description                                  | Exemple                       |
| ------------------------ | -------------------------------------------- | ----------------------------- |
| `concat()`               | Fusionne deux tableaux                       | `a.concat(b)`                 |
| `slice(start, end)`      | Extrait une portion sans modifier l'original | `arr.slice(1, 3)`             |
| `splice(start, nb, ...)` | Modifie le tableau (ajoute ou retire)        | `arr.splice(1, 2)`            |
| `reverse()`              | Inverse l’ordre                              | `arr.reverse()`               |
| `sort()`                 | Trie (attention à l’ordre lexicographique)   | `arr.sort()` ou avec callback |
| `fill(val)`              | Remplit avec une valeur                      | `new Array(3).fill("X")`      |
| `join()`                 | Transforme en chaîne                         | `arr.join(", ")`              |

**Rechercher / tester**

| Méthode       | Description                                   | Exemple                              |
| ------------- | --------------------------------------------- | ------------------------------------ |
| `indexOf()`   | Premier index d’un élément                    | `arr.indexOf("kiwi")`                |
| `includes()`  | Teste si un élément existe                    | `arr.includes("banane")`             |
| `find()`      | Retourne le **premier élément** correspondant | `arr.find(el => el.length > 5)`      |
| `findIndex()` | Index du **premier** élément correspondant    | `arr.findIndex(el => el === "kiwi")` |

**Parcourir un tableau**

✅ `for`

```js
for (let i = 0; i < fruits.length; i++) {
  console.log(fruits[i]);
}
```

✅ `for...of`

```js
for (let fruit of fruits) {
  console.log(fruit);
}
```

✅ `forEach()`

```js
fruits.forEach((fruit, index) => {
  console.log(index, fruit);
});
```

**Méthodes fonctionnelles (ES6+)**

| Méthode    | Description                            | Exemple                                 |
| ---------- | -------------------------------------- | --------------------------------------- |
| `map()`    | Transforme chaque élément              | `arr.map(x => x * 2)`                   |
| `filter()` | Garde les éléments selon une condition | `arr.filter(x => x > 3)`                |
| `reduce()` | Réduit à une seule valeur              | `arr.reduce((acc, val) => acc + val)`   |
| `some()`   | Retourne `true` si au moins un match   | `arr.some(x => x > 5)`                  |
| `every()`  | Retourne `true` si **tous** matchent   | `arr.every(x => typeof x === "number")` |

## **🎯 Exemples utiles**

**Cloner un tableau**

```js
const clone = [...fruits];
```

### 🧪 Cloner un tableau (sans `...`)

Voici plusieurs façons de **copier un tableau sans mutation** :

**Avec `.slice()`**

```js
const original = [1, 2, 3];
const clone = original.slice(); // [1, 2, 3]
```

👉 `.slice()` sans argument retourne une **copie superficielle** du tableau.

**Avec `Array.from()`**

```js
const clone = Array.from(original);
```

**Avec `.map()` identique**

```js
const clone = original.map(x => x);
```

❗ Ne pas faire :

```js
const alias = original;
alias[0] = 99;
```

⚠️ Cela ne clone pas, **cela crée une référence**, donc les deux tableaux pointent vers la même structure mémoire.

**Supprimer un élément (sans mutation)**

```js
const withoutBanana = fruits.filter(fruit => fruit !== "banane");
```

logique inversé, récupère tous les élèments sauf banane.

**Extraire les premiers éléments**

```js
const premiers = fruits.slice(0, 2); // ["pomme", "banane"]
```

**Sélectionner le dernier élément d’un tableau**

❌ Mauvaise intuition (n’existe pas nativement) :

```js
arr[-1] // undefined
```

✅ Bonne méthode :

```js
const last = arr[arr.length - 1];
```

✨ Astuce (prochaine version JavaScript) :

Avec **`.at()`** (introduit en ES2022), tu peux faire :

```js
const last = arr.at(-1);
```

✔️ Fonctionne aussi avec les **valeurs négatives** comme en Python.

**Nettoyer un tableau (vider)**

```js
arr.length = 0;
```

**Inverser tableau sans muter l’original**

```js
const reversed = [...arr].reverse();
```

**Vérifier si c’est bien un tableau**

```js
Array.isArray(arr); // true
```

**Compter les occurrences d’un élément**

```js
const fruits = ["pomme", "banane", "pomme"];
const count = fruits.filter(f => f === "pomme").length; // 2
```

**Remplir un tableau rapidement**

```js
const zeros = new Array(5).fill(0); // [0, 0, 0, 0, 0]
```

**Fusionner plusieurs tableaux**

```js
const all = [].concat(arr1, arr2, arr3);
```

✅ Ou moderne :

```js
const all = [...arr1, ...arr2, ...arr3];
```

## Tableaux vs Objets

| Caractéristique          | Tableaux                | Objets                  |
| ------------------------ | ----------------------- | ----------------------- |
| Index numérique          | Oui                     | Non (clés nommées)      |
| Ordre garanti            | Oui                     | Non (sauf ES2020+)      |
| Méthodes de manipulation | Oui (map, filter, etc.) | Peu (Object.keys, etc.) |

## Résumé

| Action             | Méthode                           |
| ------------------ | --------------------------------- |
| Ajouter un élément | `push()`, `unshift()`             |
| Retirer un élément | `pop()`, `shift()`, `splice()`    |
| Extraire ou cloner | `slice()`, `filter()`             |
| Transformer        | `map()`, `reduce()`               |
| Tester             | `includes()`, `some()`, `every()` |
| Rechercher         | `find()`, `indexOf()`             |
| Trier / inverser   | `sort()`, `reverse()`             |

## Résumé des meilleurs tips

| Action             | Astuce                                    |
| ------------------ | ----------------------------------------- |
| Cloner             | `slice()`, `Array.from()`, `map()`        |
| Dernier élément    | `arr[arr.length - 1]` ou `arr.at(-1)`     |
| Vider              | `arr.length = 0`                          |
| Vérifier tableau   | `Array.isArray(arr)`                      |
| Uniques            | `[...new Set(arr)]`                       |
| Fusionner          | `[].concat(...)` ou `[..., ...]`          |
| Alphabet dynamique | `Array.from(...String.fromCharCode(...))` |

## 📚 Liens utiles

- [MDN - Array](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Array)

- [JavaScript.info - Tableaux](https://fr.javascript.info/array)


