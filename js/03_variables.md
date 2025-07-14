## 2️⃣ `let`

### Description

- Introduit avec ES6.

- **Portée bloc** (*block-scoped*).

- Peut être **réassigné**, mais **pas redeclaré dans le même scope**.

### Exemple

```js
function exempleLet() {
  if (true) {
    let y = 20;
    console.log(y); // 20
  }
  // console.log(y); // ReferenceError
}
```

### Caractéristiques

❌ Redeclaration interdite dans le même scope  
✅ Réassignation autorisée  
✅ Portée bloc  
✅ Hoisting partiel : La déclaration est levée, mais pas l'initialisation (erreur si utilisée avant la ligne de déclaration).

---

## 3️⃣ `const`

### Description

- Introduit avec ES6.

- **Portée bloc** (*block-scoped*).

- **Doit être initialisée** à la déclaration.

- Ne peut pas être **réassignée**.

### Exemple

```js
const z = 30;
// z = 40; // TypeError: Assignment to constant variable.
```

Note : Les objets ou tableaux déclarés avec `const` peuvent voir leur contenu modifié, mais pas leur référence.

```js
const tab = [1, 2, 3];
tab.push(4); // Ok
// tab = [5,6]; // Erreur
```

### Caractéristiques

❌ Redeclaration interdite  
❌ Réassignation interdite (la référence)  
✅ Portée bloc  
✅ Hoisting partiel (comme `let`)

## Résumé Comparatif

|                | var             | let       | const           |
| -------------- | --------------- | --------- | --------------- |
| **Portée**     | Fonction        | Bloc      | Bloc            |
| **Hoisting**   | Oui (undefined) | Oui (TDZ) | Oui (TDZ)       |
| **Redeclarer** | Oui             | Non       | Non             |
| **Réassigner** | Oui             | Oui       | Non (référence) |

> **TDZ** = Temporal Dead Zone, période entre le hoisting et l'initialisation où l'accès à la variable provoque une erreur.

## Conseils de Bonnes Pratiques

✅ Privilégier `const` par défaut  
✅ Utiliser `let` si la valeur doit changer  
⚠️ Éviter `var` sauf pour raisons de compatibilité ou besoin spécifique

## Liens Utiles

- [MDN - var](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Instructions/var)

- [MDN - let](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Instructions/let)

- [MDN - const](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Instructions/const)

- [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS)

## 🏗️ Définition du Hoisting

**Hoisting** *(en français : levage)* est un mécanisme interne de JavaScript qui consiste à **déplacer les déclarations de variables et de fonctions en haut de leur scope** (portée) **avant l’exécution du code**.

En d’autres termes :

> JavaScript **traite d’abord toutes les déclarations** lors de la phase de compilation, **puis exécute le code**.

🎯 Exemple avec `var`

```js
console.log(a); // undefined
var a = 5;
console.log(a); // 5

```

**Explication :**  
Le moteur JavaScript interprète ce code comme si tu avais écrit :

```js
var a;
console.log(a); // undefined
a = 5;
console.log(a); // 5

```

La **déclaration** (`var a;`) est levée en haut du scope, mais **l’affectation** (`a = 5`) reste en place.

.

---

### 🎯 Exemple avec `let` ou `const`

javascript

```js
console.log(b); // ReferenceError
let b = 10;
```


