# Typage et Nommage des Variables en JavaScript

### 📚 Types Primitifs

JavaScript possède **7 types primitifs** principaux :

| Type      | Exemple                      |
| --------- | ---------------------------- |
| Number    | `42`, `3.14`                 |
| String    | `"Hello"`                    |
| Boolean   | `true`, `false`              |
| Undefined | variable non initialisée     |
| Null      | absence volontaire de valeur |
| Symbol    | identifiant unique (ES6)     |
| BigInt    | grands entiers (ES2020)      |

**Attention** : `typeof null` retourne `"object"` (c’est un bug historique).

### 📦 Types Complexes

- **Object** : collections de paires clé-valeur

- **Array** : tableaux indexés

- **Function** : fonctions (objets exécutables)
  
  

## 🖊️ 2. Le Nommage des Variables

Le nommage est **sensible à la casse** et doit suivre certaines règles.

### ✅ Règles Syntaxiques

- Doit commencer par une lettre, `$` ou `_`

- Ne peut pas commencer par un chiffre

- Peut contenir des lettres, chiffres, `$` ou `_`

- Ne doit pas être un mot réservé (comme `if`, `while`, `return`...)

```js
let maVariable = 10;
let $compteur = 0;
let _resultat = "ok";
```

### 🧭 Bonnes Pratiques

✔️ Utiliser la **camelCase** (par convention JavaScript)  
✔️ Choisir des noms **clairs et explicites**  
✔️ Éviter les abréviations inutiles  
✔️ Préférer l’anglais (langue majoritaire des API et de la communauté)

#### 🎯 Exemples

```js
let nombreUtilisateur = 5;    // ok
let userCount = 5;            // ok
let nbUsr = 5;                // moins clair
```

🔑 Sensibilité à la casse

```js
let total = 10;
let Total = 20;
console.log(total); // 10
console.log(Total); // 20
```

## 🧪 3. Vérifier le Type

Pour connaître le type d’une variable, utiliser `typeof` :

```js
let x = "Hello";
console.log(typeof x); // "string"

x = 42;
console.log(typeof x); // "number"
```

## 📝 Résumé

✅ JavaScript est **dynamiquement typé**  
✅ Les variables changent de type au cours du programme  
✅ Respecter les conventions de nommage pour un code lisible  
✅ Utiliser `typeof` pour vérifier le type

## 📚 Liens Utiles

- [MDN - Types](https://developer.mozilla.org/fr/docs/Web/JavaScript/Data_structures)

- [MDN - typeof](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Operators/typeof)

- [JavaScript Guide - Nommage](https://developer.mozilla.org/fr/docs/Glossaire/Identificateur)


