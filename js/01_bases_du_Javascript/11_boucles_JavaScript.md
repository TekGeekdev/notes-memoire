# 🔁 Les Boucles en JavaScript

## 🧭 Pourquoi utiliser des boucles ?

En programmation, on rencontre **souvent des situations où une tâche doit être répétée** plusieurs fois :

- parcourir des éléments d’un tableau

- lire les propriétés d’un objet

- répéter un calcul

- filtrer ou transformer une liste

- attendre une condition avant d'agir

Les **boucles permettent de gagner du temps et d’automatiser les traitements**. Elles sont essentielles dans **tous les langages**, et JavaScript en propose plusieurs, chacune adaptée à un contexte particulier.

## 📌 Quand utiliser quelle boucle ?

| Boucle       | Quand l’utiliser                                                                   |
| ------------ | ---------------------------------------------------------------------------------- |
| `for`        | Quand on **sait combien de fois** on doit répéter                                  |
| `while`      | Quand on **ne connaît pas à l’avance** le nombre d’itérations                      |
| `do...while` | Quand on veut **exécuter une action au moins une fois**, puis tester une condition |
| `for...of`   | Pour **parcourir les éléments** d’un tableau, une chaîne, ou tout itérable         |
| `for...in`   | Pour **parcourir les propriétés d’un objet**                                       |
| `forEach()`  | Pour appliquer une fonction **à chaque élément d’un tableau**                      |

## `for` — Boucle avec incrémentation **manuelle et explicite**

**Syntaxe :**

```js
for (initialisation; condition; incrément) {
  // bloc de code
}
```

### ✍️ Ce que tu dois faire :

- Tu **initialises** une variable (ex. `let i = 0`)

- Tu écris une **condition** (tant qu'elle est vraie, la boucle continue)

- Tu **incrémentes** manuellement (`i++`, `i += 2`, etc.)

### 🔧 Incrémentation ?

➡️ **Oui, c’est toi qui la codes** dans le 3e bloc.

Exemple 1 — Afficher les 10 premiers nombres

```js
for (let i = 1; i <= 10; i++) {
  console.log(i);
}
```

Exemple 2 — Multiplier chaque élément d’un tableau

```js
const nombres = [1, 2, 3];
for (let i = 0; i < nombres.length; i++) {
  nombres[i] *= 2;
}
console.log(nombres); // [2, 4, 6]
```

## `while` — Boucle tant que c’est vrai, **sans incrémentation automatique**

La boucle `while` est utile quand **on ne sait pas combien de fois** une action devra être répétée.  
Elle est parfaite pour :

- attendre une condition aléatoire

- lire un flux de données

- interagir avec l'utilisateur jusqu'à une réponse valide

**Syntaxe :**

```js
let i = 0;
while (condition) {
  // bloc de code
  i++; // incrémentation manuelle obligatoire
}
```

### 🔧 Incrémentation ?

➡️ **Non automatique**. Tu dois **incrémenter manuellement** dans le corps de la boucle, sinon tu risques une **boucle infinie**.

Exemple 1 — Attendre un tirage précis

```js
let n = 0;
while (n !== 3) {
  n = Math.floor(Math.random() * 5) + 1;
  console.log("Tirage :", n);
}
```

Exemple 2 — Attendre une saisie utilisateur correcte

```js
let code;
while (code !== "1234") {
  code = prompt("Code secret ?");
}
```

## `do...while` — Pareil que `while`, mais **exécutée au moins une fois**

Contrairement à `while`, la boucle `do...while` **garantit au moins une exécution**.  
Elle est utile lorsque l’on veut :

- afficher un menu au moins une fois

- forcer une première action avant de vérifier

- effectuer des essais avec retour

**Syntaxe :**

```js
let i = 0;
do {
  // bloc de code
  i++; // incrémentation manuelle nécessaire
} while (condition);
```

### 🔧 Incrémentation ?

➡️ **Non automatique**. Tu dois aussi **incrémenter manuellement**.

Exemple 1 — Une tentative obligatoire

```js
let password;
do {
  password = prompt("Mot de passe :");
} while (password !== "admin");
```

Exemple 2 — Tirage aléatoire unique puis répétition

```js
let n;
do {
  n = Math.floor(Math.random() * 10);
  console.log("Essai :", n);
} while (n < 5);
```

## `for...of` — boucle moderne et lisible pour les tableaux

Introduite en ES6, `for...of` est conçue pour **parcourir directement les valeurs** d’un tableau ou d’un itérable (chaîne, Set, Map...).

Utilisation typique :

- parcourir des tableaux sans se soucier de l’index

- lire une chaîne caractère par caractère

- agréger des valeurs

**Syntaxe :**

```js
for (let valeur of iterable) {
  // bloc de code
}
```

### 🔧 Incrémentation ?

➡️ **Automatique et interne**. Tu **n’as rien à faire** : JavaScript avance dans le tableau pour toi.

Exemple 1 — Parcourir un tableau

```js
const fruits = ["pomme", "banane", "kiwi"];
for (let fruit of fruits) {
  console.log(fruit);
}
```

Exemple 2 — Additionner des notes

```js
const notes = [10, 14, 12];
let total = 0;
for (let note of notes) {
  total += note;
}
```

## `for...in` — pour les objets

La boucle `for...in` est idéale pour **parcourir les propriétés (clés)** d’un objet.  
Elle est utile pour :

- lire les données d’un objet

- construire dynamiquement des affichages

- déboguer une structure inconnue

**Syntaxe :**

```js
for (let clé in objet) {
  // bloc de code
}
```

### 🔧 Incrémentation ?

➡️ **Automatique et implicite**. Tu **n’as rien à faire** : chaque tour te donne la prochaine **clé** de l’objet.

Exemple 1 — Lire les propriétés

```js
const user = { nom: "Alice", age: 30 };
for (let prop in user) {
  console.log(`${prop} : ${user[prop]}`);
}
```

Exemple 2 — Vérifier des propriétés spécifiques

```js
for (let key in user) {
  if (key.startsWith("n")) {
    console.log("Propriété commençant par n :", key);
  }
}
```

## `forEach()` — fonctionnelle et pratique

`.forEach()` est une **méthode d’objet Array**.  
Très utilisée en JavaScript moderne, notamment en front-end :

- pour manipuler des listes de données (cartes, produits…)

- pour itérer avec une **callback**

- quand on n’a **pas besoin de return**

**Syntaxe :**

```js
array.forEach((valeur, index) => {
  // bloc de code
});
```

### 🔧 Incrémentation ?

➡️ **Gérée automatiquement** par JavaScript. Tu **n’as pas à gérer** l’index.

Exemple 1 — Afficher avec l’index

```js
const jours = ["lundi", "mardi", "mercredi"];
jours.forEach((jour, i) => console.log(i, jour));
```

Exemple 2 — Calcul d’un total

```js
const prix = [10, 20, 30];
let total = 0;
prix.forEach(p => total += p);
console.log(total); // 60
```

## 🔧 7. Contrôle avec `break` et `continue`

### `break` : interrompt complètement la boucle

```js
for (let i = 0; i < 10; i++) {
  if (i === 4) break;
  console.log(i);
}
```

`continue` : saute à l’itération suivante

```js
for (let i = 1; i <= 5; i++) {
  if (i % 2 === 0) continue;
  console.log(i); // affiche seulement les impairs
}
```

## Résumé

| Boucle       | Utilisation principale                                  |
| ------------ | ------------------------------------------------------- |
| `for`        | Compter, indexer, répéter un nombre fixe de fois        |
| `while`      | Répéter une action jusqu’à ce qu’une condition change   |
| `do...while` | Exécuter au moins une fois puis tester                  |
| `for...of`   | Parcourir les valeurs d’un tableau, chaîne, Set, Map... |
| `for...in`   | Parcourir les **clés** d’un objet                       |
| `forEach()`  | Exécuter une fonction sur chaque élément d’un tableau   |

| Boucle       | Incrémentation          | Qui la fait ?           | Tu dois écrire `i++` ? |
| ------------ | ----------------------- | ----------------------- | ---------------------- |
| `for`        | ❗ Obligatoire           | **Toi** (dans le `for`) | ✅ Oui                  |
| `while`      | ❗ Obligatoire           | **Toi** (dans le bloc)  | ✅ Oui                  |
| `do...while` | ❗ Obligatoire           | **Toi** (dans le bloc)  | ✅ Oui                  |
| `for...of`   | ✅ Automatique           | **JavaScript**          | ❌ Non                  |
| `for...in`   | ✅ Automatique           | **JavaScript**          | ❌ Non                  |
| `forEach()`  | ✅ Automatique (tableau) | **JavaScript**          | ❌ Non                  |



## 📚 Liens utiles

- [MDN - for](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Statements/for)

- [MDN - while](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Statements/while)

- [MDN - do...while](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Statements/do...while)

- [MDN - forEach()](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)

- [MDN - for...in](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Statements/for...in)

- [MDN - for...of](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Statements/for...of)


