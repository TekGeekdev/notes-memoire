# 🌟Les conditions `if`, les valeurs truthy et falsy en JavaScript



## 🧠 1. C’est quoi une condition ?

Une condition permet d’exécuter un bloc de code **seulement si une certaine expression est vraie**.

En JavaScript, la structure de base est :

```js
if (condition) {
  // code exécuté si la condition est vraie
}
```

**Exemple simple :**

```js
let age = 18;

if (age >= 18) {
  console.log("Tu es majeur !");
}
```

## 🔀 2. Ajout d’alternatives : `else` et `else if`

**`else` permet de gérer le cas où la condition est fausse** :

```js
let isLoggedIn = false;

if (isLoggedIn) {
  console.log("Bienvenue !");
} else {
  console.log("Connecte-toi d'abord.");
}
```

**`else if` permet de tester plusieurs cas :**

```js
let note = 85;

if (note >= 90) {
  console.log("Excellent !");
} else if (note >= 75) {
  console.log("Très bien !");
} else if (note >= 60) {
  console.log("Passable.");
} else {
  console.log("Échec.");
}
```

## ✅ 3. Les valeurs **truthy** et **falsy**

Quand JavaScript évalue une condition, il convertit la valeur en **booléen implicite**. On parle alors de valeurs **truthy** ou **falsy**.

**❗ Falsy values (considérées comme fausses)**

Ces valeurs **équivalent à `false`** lorsqu’elles sont évaluées dans une condition :

| Valeur      | Description        |
| ----------- | ------------------ |
| `false`     | le booléen faux    |
| `0`         | zéro (nombre)      |
| `-0`        | zéro négatif       |
| `0n`        | zéro en BigInt     |
| `""`        | chaîne vide        |
| `null`      | absence de valeur  |
| `undefined` | valeur non définie |
| `NaN`       | Not a Number       |

Exemple :

```js
let username = "";

if (username) {
  console.log("Bienvenue, " + username);
} else {
  console.log("Aucun nom d'utilisateur");
}
// Résultat : "Aucun nom d'utilisateur"
```

**✅ Truthy values (considérées comme vraies)**

Toutes les **autres valeurs** sont considérées comme **vraies**, même si elles paraissent "vides" :

| Exemple de valeur | Résultat |
| ----------------- | -------- |
| `"0"`             | truthy   |
| `"false"`         | truthy   |
| `[]`              | truthy   |
| `{}`              | truthy   |
| `function() {}`   | truthy   |

Exemple :

```js
let data = [];

if (data) {
  console.log("Le tableau existe !");
}
// Résultat : "Le tableau existe !" (même s’il est vide)
```

#### **Exemple complet**

```js
let user = {
  name: "Alice",
  age: 0
};

if (user.name) {
  console.log("Nom : " + user.name);
}

if (user.age) {
  console.log("Âge : " + user.age);
} else {
  console.log("Âge inconnu");
}
```

**Résultat :**

```js
Nom : Alice
Âge inconnu
```

👉 `user.age` vaut `0`, donc c’est falsy, même si c’est une valeur "valide".



## ⚠️ 4. Bonnes pratiques

- Toujours vérifier explicitement si c’est ce que tu veux comparer :

```js
if (user.age !== undefined) {
  console.log("Âge : " + user.age);
}
```

- Pour vérifier qu’une chaîne n’est **ni vide, ni `null`, ni `undefined`** :

```js
if (myString) { ... }
```



## 🧩 5. Syntaxe abrégée : opérateur ternaire

Format court d’un `if...else` :

```js
let message = (age >= 18) ? "Majeur" : "Mineur";
```

Si majeur dit (?) "Majeur" sinon (:) "Mineur"



## 📌 Résumé

| Élément          | Description                               |
| ---------------- | ----------------------------------------- |
| `if (condition)` | Exécute un bloc si la condition est vraie |
| `else`           | Sinon, exécute un autre bloc              |
| `else if`        | Teste une deuxième condition              |
| `truthy`         | Valeurs considérées comme `true`          |
| `falsy`          | Valeurs considérées comme `false`         |
| `? :` (ternaire) | Écriture courte de `if...else`            |



#### 📚 Liens utiles

- [if…else - JavaScript | MDN](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Statements/if...else)

- [opérateur conditionnel - JavaScript | MDN](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Operators/Conditional_operator)

- [Truthy - Glossary | MDN](https://developer.mozilla.org/en-US/docs/Glossary/Truthy)

- [Falsy - Glossary | MDN](https://developer.mozilla.org/en-US/docs/Glossary/Falsy)


