# ✨ Expressions et Instructions en JavaScript

## 🟢 **Instruction** (*Statement*)

Une **instruction** est une ligne de code qui **effectue une action complète**.  
Elle se termine généralement par un **point-virgule (`;`)** (même si JavaScript peut l’insérer automatiquement).

👉 **Exemple d’instructions :**

- Déclarer une variable

- Faire une boucle

- Exécuter une condition

✅ **Exemple :**

```js
let x = 5;           // Déclaration (instruction)
if (x > 3) {         // Instruction conditionnelle
  console.log(x);    // Instruction
}
```

## 🟢 **Expression**

Une **expression** est une partie de code qui produit **une valeur**.

✅ On peut la **stocker dans une variable**, **l’utiliser comme argument**, ou **la retourner**.

✅ **Exemples d’expressions :**

- `2 + 2`

- `"Hello"`

- `x * y`

- `true`

- `myFunction()`

```js
let y = 2 + 2;       // "2 + 2" est une expression
console.log(y);      // Affiche 4
```

### 🔍 Différence simple

| Type        | Fait quoi ?         |
| ----------- | ------------------- |
| Instruction | Effectue une action |
| Expression  | Produit une valeur  |

### 🎯 Exemples Concrets

**✅ Exemples d’instructions**

```js
let a = 10;                // Instruction de déclaration
if (a > 5) {               // Instruction conditionnelle
  console.log("ok");       // Instruction d'appel
}
for (let i = 0; i < 3; i++) { // Instruction de boucle
  console.log(i);
}
```

**✅ Exemples d’expressions**

```js
3 + 4                   // expression arithmétique
"Hello"                 // expression littérale
myFunction()            // expression d'appel de fonction
x * y                   // expression produit
true                    // expression booléenne
```

**✅ Exemples mixtes**

Tu peux avoir des instructions qui contiennent des expressions :

```js
let total = 3 + 4;      // "3 + 4" est une expression, l'ensemble est une instruction
```

### 🧠 Les Expressions d’Assignation

Quand tu écris :

```js
let z = 10;
```

✅ C’est une **instruction d’assignation**.  
✅ Mais la partie `10` est une **expression**.  
✅ Le tout est **une instruction complète**.

## ✨ Expressions qui retournent des valeurs

Certaines expressions sont auto-évaluées :

```js
2 + 2          // retourne 4
"Hello"        // retourne "Hello"
true           // retourne true
```

✅ Tu peux les passer à `console.log` :

```js
console.log(2 + 2);
```

## Tableau comparatif

| Exemple              | Expression ?                                     | Instruction ?          |
| -------------------- | ------------------------------------------------ | ---------------------- |
| `2 + 2`              | ✅                                                | ❌                      |
| `let x = 2 + 2;`     | ✅ (à droite),                                    | ✅ globalement          |
| `if (x > 3) { ... }` | ❌ (globalement), contient une expression `x > 3` | ✅                      |
| `console.log("ok");` | ✅ (appel de fonction),                           | ✅ instruction complète |
| `"Hello"`            | ✅                                                | ❌                      |

## 💡 Bonnes Pratiques

✅ Les instructions structurent votre programme (déclarations, contrôles, boucles)  
✅ Les expressions produisent les valeurs à manipuler  
✅ Une instruction peut contenir une ou plusieurs expressions  
✅ En JavaScript, **tout n’est pas expression** contrairement à d’autres langages comme Ruby ou Lisp

## 📝 Résumé

- ✅ **Instruction** : Fait une action (déclare, exécute, contrôle)

- ✅ **Expression** : Produit une valeur

- ✅ Les instructions peuvent contenir des expressions

---

## 🧪 Exercice simple

**Identifier si c’est une expression, une instruction ou les deux :**

1. `x + y`

2. `let a = 5;`

3. `if (a > 3) { ... }`

4. `"Hello"`

5. `console.log("Hello");`

*(Réponses : 1=Expression, 2=Les deux, 3=Instruction avec expression, 4=Expression, 5=Les deux)*

---

## 📚 Liens utiles

- [MDN - Expressions](https://developer.mozilla.org/fr/docs/Web/JavaScript/Guide/Expressions_et_Op%C3%A9rateurs)

- [MDN - Instructions](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Statements)


