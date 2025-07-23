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



## Bonus

### Décomposition des conditions complexes

#### Objectif

En programmation, on écrit souvent des **conditions `if` complexes** avec plusieurs opérateurs logiques (`&&`, `||`, `!`).  
Mais plus la condition est longue, plus elle devient difficile à lire, à comprendre ou à corriger.

👉 L’objectif est donc de **décomposer** une condition complexe en **sous-conditions nommées** pour améliorer la **lisibilité**, la **maintenance** et le **débogage** du code.

😵 Exemple de condition complexe

```js
if ((user.isLoggedIn && user.role === 'admin' && user.accountActive) || user.isSuperAdmin) {
  showDashboard();
}
```

### ❌ Problèmes :

- Difficile à lire rapidement.

- Plusieurs responsabilités dans une seule ligne.

- Difficile à tester ou modifier sans casser quelque chose.

## 🛠️ Décomposition en sous-conditions

On peut **isoler** chaque partie dans une constante bien nommée :

```js
const isRegularAdmin = user.isLoggedIn && user.role === 'admin' && user.accountActive;
const hasAccess = isRegularAdmin || user.isSuperAdmin;

if (hasAccess) {
  showDashboard();
}
```

### ✅ Avantages :

| Avant              | Après                                      |
| ------------------ | ------------------------------------------ |
| Long et confus     | Clair et organisé                          |
| Pas réutilisable   | Les constantes peuvent être réutilisées    |
| Difficile à tester | On peut tester chaque condition séparément |



### 🧪 Autre exemple : formulaire d’inscription

**🧩 Condition complexe**

```js
if (user.age >= 18 && user.email.includes("@") && user.password.length >= 8 && !user.isBanned) {
  registerUser();
}
```

**✅ Décomposée pour plus de clarté :**

```js
const isAdult = user.age >= 18;
const hasValidEmail = user.email.includes("@");
const hasStrongPassword = user.password.length >= 8;
const isAllowed = !user.isBanned;

const canRegister = isAdult && hasValidEmail && hasStrongPassword && isAllowed;

if (canRegister) {
  registerUser();
}
```

## 📝 Conclusion

🔹 **Mieux vaut plusieurs petites conditions bien nommées qu’une grosse condition illisible.**  
🔹 Cela permet de rendre le code **plus clair**, **plus testable** et **plus facile à maintenir**.



### 📚 Liens utiles bonus

[Intermediate to advanced content level Clean code Book](https://www.oreilly.com/library/view/clean-code-a/9780136083238/)


