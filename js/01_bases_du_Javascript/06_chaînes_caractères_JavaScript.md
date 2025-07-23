# ✨ Les chaînes de caractères en JavaScript

## 📖 Qu’est-ce qu’une chaîne ?

Une **chaîne de caractères** (*string*) est une suite de caractères entourée par :

- des guillemets doubles `"`

- des guillemets simples `'`

- ou des backticks `` ` `` (littéraux de gabarits)

✅ Exemples de déclarations

```js
let s1 = "Bonjour";
let s2 = 'Hello';
let s3 = `Salut`;
```

## 📝 Caractéristiques principales

- Les chaînes sont **immutables** (on ne peut pas modifier leur contenu directement).

- Les chaînes sont **indexées** (chaque caractère a un indice).

- Les chaînes ont des **méthodes intégrées** puissantes.

## 🔢 La longueur d’une chaîne

**Propriété `.length`**

```js
let message = "Hello";
console.log(message.length); // 5
```

## 🧩 Accéder à un caractère

✅ Avec les crochets `[]`

```js
console.log(message[0]); // "H"
```

✅ Avec `.charAt()`

```js
console.log(message.charAt(1)); // "e"
```

## 🔡 Concaténation

**Opérateur `+`**

```js
let greet = "Hello, " + "world!";
```



## Les littéraux de gabarits (*template literals*)

### 📖 Qu’est-ce que c’est ?

Les **littéraux de gabarits** sont une façon moderne d’écrire des chaînes :  
✅ Plus lisible  
✅ Multi-lignes  
✅ Interpolation directe de variables

Ils utilisent des **backticks** `` ` `` au lieu des guillemets `'` ou `"`.

**Syntaxe de base**

```js
let texte = `Ceci est un template literal`;
```

**Interpolation de variables**

**Avec `${}`**, on peut insérer des variables ou expressions :

```js
let nom = "Alice";
let message = `Bonjour, ${nom}!`;
console.log(message); // "Bonjour, Alice!"
```

**Calculs directement dans la chaîne** :

```js
let a = 2;
let b = 3;
console.log(`Le résultat est ${a + b}`); // "Le résultat est 5"
```

**Chaîne multi-lignes**

```js
let poem = `Roses are red,
Violets are blue,
JavaScript is awesome,
And so are you.`;

console.log(poem);
```

**Expressions complexes**

```js
function getGreeting(name) {
  return `Hello, ${name.toUpperCase()}!`;
}

console.log(getGreeting("bob")); // "Hello, BOB!"
```

⚠ **Attention** : les backticks ne sont pas des apostrophes `'` ni des guillemets `"`.  
Ils se trouvent en haut à gauche du clavier (sous `Échap`).



## 🔄 Changements de casse

```js
let word = "JavaScript";
console.log(word.toUpperCase()); // "JAVASCRIPT"
console.log(word.toLowerCase()); // "javascript"
```

## ✂️ Extraction de sous-chaînes

| Méthode                  | Exemple                  | Résultat |
| ------------------------ | ------------------------ | -------- |
| `.slice(start, end)`     | `"Hello".slice(1,4)`     | "ell"    |
| `.substring(start, end)` | `"Hello".substring(1,4)` | "ell"    |
| `.substr(start, length)` | `"Hello".substr(1,3)`    | "ell"    |

## 

## 🔍 Recherche dans une chaîne

| Méthode         | Exemple                                  |
| --------------- | ---------------------------------------- |
| `indexOf()`     | `"Hello".indexOf("e")` → 1               |
| `lastIndexOf()` | `"banana".lastIndexOf("a")` → 5          |
| `includes()`    | `"JavaScript".includes("Script")` → true |
| `startsWith()`  | `"Hello".startsWith("He")` → true        |
| `endsWith()`    | `"Hello".endsWith("lo")` → true          |



## 🔄 Remplacement

```js
let str = "Bonjour Alice";
let newStr = str.replace("Alice", "Bob");
console.log(newStr); // "Bonjour Bob"
```

✅ Pour remplacer **toutes les occurrences** :

```js
let text = "a b a b";
let result = text.replace(/a/g, "X"); // "X b X b"
```



## 🧩 Découper une chaîne

Méthode `.split()`

```js
let phrase = "un,deux,trois";
let parts = phrase.split(",");
console.log(parts); // ["un", "deux", "trois"]
```



## Répéter une chaîne

```js
"ha".repeat(3); // "hahaha"
```



## 🧹 Nettoyer les espaces

```js
"   hello   ".trim();       // "hello"
"   hello   ".trimStart();  // "hello   "
"   hello   ".trimEnd();    // "   hello"
```



### ✅ Exemples combinés

```js
let name = "Bob";
let s = `  JavaScript for ${name} `;
s = s.trim().toUpperCase();
console.log(s); // "JAVASCRIPT FOR BOB"
```



## 📝 Résumé rapide des méthodes utiles

| Action                        | Méthode                            |
| ----------------------------- | ---------------------------------- |
| Longueur                      | `.length`                          |
| Extraire une portion          | `.slice()`, `.substring()`         |
| Convertir majuscule/minuscule | `.toUpperCase()`, `.toLowerCase()` |
| Chercher un texte             | `.indexOf()`, `.includes()`        |
| Remplacer un texte            | `.replace()`                       |
| Découper en tableau           | `.split()`                         |
| Répéter                       | `.repeat()`                        |
| Nettoyer les espaces          | `.trim()`                          |
| Interpolation                 | `` `Bonjour, ${name}!` ``          |

---

## 📚 Liens utiles

- [MDN - String](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/String)

- [MDN - Template literals](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Template_literals)


