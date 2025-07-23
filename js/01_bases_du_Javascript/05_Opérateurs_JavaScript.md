# ✨ Les Opérateurs en JavaScript

## 📖 Définition

Un **opérateur** est un symbole ou un mot qui permet de **manipuler des valeurs** et **produire un résultat**.

## 📋 Catégories principales

JavaScript propose plusieurs types d’opérateurs :

1. **Opérateurs arithmétiques**

2. **Opérateurs d’assignation**

3. **Opérateurs de comparaison**

4. **Opérateurs logiques**

5. **Opérateurs bit-à-bit**

6. **Opérateurs de chaîne**

7. **Opérateurs conditionnels**

8. **Opérateurs de type**

9. **Opérateurs de décomposition et propagation**



### Opérateurs arithmétiques

| Opérateur | Description    | Exemple       |
| --------- | -------------- | ------------- |
| `+`       | Addition       | `2 + 3 // 5`  |
| `-`       | Soustraction   | `5 - 2 // 3`  |
| `*`       | Multiplication | `3 * 4 // 12` |
| `/`       | Division       | `10 / 2 // 5` |
| `%`       | Modulo (reste) | `7 % 3 // 1`  |
| `**`      | Exponentiation | `2 ** 3 // 8` |
| `++`      | Incrément (+1) | `x++`         |
| `--`      | Décrément (-1) | `x--`         |

#### ➗🟰 L'opérateur **modulo** (`%`)

### 🧠 **Définition simple :**

L'opérateur **modulo** (`%`) donne le **reste** d’une division entière.

📌 **Formule :**

```js
a % b
```

➡ Cela signifie : « combien reste-t-il quand on divise `a` par `b` ? »

✅ **Exemples :**

Exemple 1 :

```js
7 % 3 // = 1
```

**Explication :**

- 7 divisé par 3 = 2 (quotient entier)

- 2 × 3 = 6

- 7 − 6 = **1**  
  ➡ Donc `7 % 3 = 1`

Exemple 2 :

```js
10 % 5 // = 0
```

**Explication :**

- 10 divisé par 5 = 2 (exact)

- Pas de reste  
  ➡ Donc `10 % 5 = 0`



## Opérateurs d’assignation

| Opérateur | Équivalent | Exemple |
| --------- | ---------- | ------- |

| `=` | Assignation simple | `x = 5` |
| --- | ------------------ | ------- |

| `+=` | Addition et assignation | `x += 3` (x = x+3) |
| ---- | ----------------------- | ------------------ |

| `-=` | Soustraction et assignation | `x -= 2` |
| ---- | --------------------------- | -------- |

| `*=` | Multiplication et assign. | `x *= 2` |
| ---- | ------------------------- | -------- |

| `/=` | Division et assignation | `x /= 2` |
| ---- | ----------------------- | -------- |

| `%=` | Modulo et assignation | `x %= 2` |
| ---- | --------------------- | -------- |

| `**=` | Exponentiation assignation | `x **= 3` |
| ----- | -------------------------- | --------- |

✅ **Exemple**

```js
let x = 4;
x *= 3; // x = 12
```



## Opérateurs de comparaison

| Opérateur | Description                     | Exemple           |
| --------- | ------------------------------- | ----------------- |
| `==`      | Égalité (conversion de type)    | `2 == "2"` true   |
| `===`     | Égalité stricte (type + valeur) | `2 === "2"` false |
| `!=`      | Différent (conversion)          | `2 != "3"` true   |
| `!==`     | Différent strict                | `2 !== "2"` true  |
| `>`       | Supérieur                       | `5 > 3` true      |
| `<`       | Inférieur                       | `5 < 3` false     |
| `>=`      | Supérieur ou égal               | `5 >= 5` true     |
| `<=`      | Inférieur ou égal               | `3 <= 4` true     |

✅ **Bonnes pratiques**  
Toujours préférer `===` et `!==` pour éviter les surprises.



## Opérateurs logiques

| Opérateur | Nom         | Exemple         | Résultat |
| --------- | ----------- | --------------- | -------- |
| `&&`      | ET logique  | `true && false` | `false`  |
| `\||`     | OU logique  | `true && false` | `true`   |
| `!`       | NON logique | `!true`         | `false`  |

**✅ Exemple**

```js
let a = true;
let b = false;

console.log(a && b); // false
console.log(a || b); // true
console.log(!a);     // false
```



## Opérateurs bit-à-bit

Ces opérateurs manipulent les bits :

| Opérateur | Description                 |
| --------- | --------------------------- |
| `&`       | ET bit-à-bit                |
| `\|`      | OU bit-à-bit                |
| `^`       | OU exclusif (XOR)           |
| `~`       | NON bit-à-bit               |
| `<<`      | Décalage à gauche           |
| `>>`      | Décalage à droite signé     |
| `>>>`     | Décalage à droite non signé |

✅ Peu utilisés au quotidien sauf cas spécifiques.



## Opérateurs de concaténation de chaînes

| Opérateur | Description           |
| --------- | --------------------- |
| `+`       | Concaténation         |
| `+=`      | Concaténation assign. |

✅ **Exemple**

```js
let s = "Hello " + "world";
s += "!";
console.log(s); // "Hello world!"
```



## Opérateur conditionnel (ternaire)

Syntaxe :

```js
condition ? valeurSiVrai : valeurSiFaux
```

✅ **Exemple**

```js
let age = 18;
let status = (age >= 18) ? "Majeur" : "Mineur";
```



## Opérateurs de décomposition et propagation

Ces opérateurs permettent de **décomposer**, **copier** ou **fusionner** des tableaux et objets.

### **Décomposition** (*Destructuring*)

> **Définition**  
> La **décomposition** consiste à **extraire des valeurs** d’un tableau ou d’un objet pour les stocker directement dans des variables distinctes.

✅ C’est une façon plus courte et plus lisible d’assigner plusieurs variables d’un coup.

Exemple tableau :

```js
const fruits = ["apple", "banana"];
const [first, second] = fruits;

console.log(first); // "apple"
console.log(second); // "banana"
```

Exemple objet :

```js
const user = { name: "Alice", age: 30 };
const { name, age } = user;

console.log(name); // "Alice"
```

### **Propagation** (*Spread operator*)

> **Définition**  
> La **propagation** permet de **déplier** (ou **étendre**) le contenu d’un tableau ou d’un objet, par exemple pour :  
> ✔️ Copier les valeurs  
> ✔️ Fusionner des tableaux ou objets  
> ✔️ Ajouter des éléments ou propriétés

✅ Le symbole est `...`

Exemple  copie tableau :

```js
const numbers = [1, 2, 3];
const copy = [...numbers];

console.log(copy); // [1, 2, 3]
```

Exemple fusion tableau :

```js
const more = [4, 5];
const all = [...numbers, ...more];

console.log(all); // [1, 2, 3, 4, 5]
```

Exemple objet copie/fusion :

```js
const person = { name: "Alice" };
const clonePerson = { ...person };
const extended = { ...person, age: 30 };

console.log(extended); // { name: "Alice", age: 30 }
```



✅ Pour te souvenir simplement :

- **Décomposition = extraire** des valeurs.

- **Propagation = étendre/copier/fusioner** les valeurs quelque part.



## 📝 Résumé des bonnes pratiques

✅ Privilégier `===` et `!==` pour comparer  
✅ Bien connaître `&&`, `||` et `!`  
✅ Utiliser le ternaire avec parcimonie pour des conditions simples  
✅ Utiliser `...` pour les copies d’objets ou de tableaux

---

## 📚 Liens utiles

- [MDN - Opérateurs](https://developer.mozilla.org/fr/docs/Web/JavaScript/Guide/Expressions_et_Op%C3%A9rateurs)

- J[avaScript.info - Operators](https://javascript.info/operators)


