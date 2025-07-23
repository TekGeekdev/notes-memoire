# La coercition de type en JavaScript

## 🧠 1. Qu’est-ce que la coercition de type ?

La **coercition de type** (ou **conversion automatique**) est un mécanisme de JavaScript qui **convertit automatiquement une valeur d’un type à un autre** quand c’est nécessaire.

Cela se produit souvent dans :

- les **conditions `if`**

- les **opérations (`+`, `==`, etc.)**

- les **comparaisons**



## 2. Coercition explicite vs implicite

| Type de coercition | Exemple                    | Description                                            |
| ------------------ | -------------------------- | ------------------------------------------------------ |
| **Explicite**      | `String(123)` → `"123"`    | conversion faite **volontairement** par le développeur |
| **Implicite**      | `123 + "abc"` → `"123abc"` | conversion faite **automatiquement** par JavaScript    |



## 3. Coercition en contexte booléen

Quand une valeur est évaluée dans un contexte booléen (comme dans un `if`), JavaScript la **convertit automatiquement** en `true` ou `false`.

C’est là que les notions de **truthy** et **falsy** entrent en jeu (déjà vues dans la note précédente).

```js
if ("") {
  console.log("truthy");
} else {
  console.log("falsy"); // affiché
}
```



## 4. Coercition en opérations arithmétiques

### ➕ Addition (`+`)

Si l’un des deux opérandes est une **chaîne**, JavaScript **convertit l’autre en chaîne** :

```js
let a = 5 + "2";    // "52"
let b = "Hello " + true;  // "Hello true"
```

### ➖ Soustraction, multiplication, division (`-`, `*`, `/`)

JavaScript convertit les **chaînes en nombres** si possible :

```js
"10" - 2     // 8
"3" * "2"    // 6
"abc" - 1    // NaN (pas un nombre)
```



## 5. Comparaison avec coercition : `==` vs `===`

### `==` (égalité **lâche**)

Effectue une **coercition automatique** de type **avant de comparer** :

```js
0 == false      // true
"123" == 123    // true
null == undefined // true
```

### `===` (égalité **stricte**)

Ne fait **aucune conversion**. Les types doivent être **identiques** :

```js
0 === false     // false
"123" === 123   // false
null === undefined // false
```

👉 **Toujours préférer `===`** en développement professionnel pour éviter les effets de bord.



## 🧠 6. Cas pratiques en condition `if`

```js
let score = "0";

if (score) {
  console.log("Score existe !");
} else {
  console.log("Score vide ou falsy.");
}
// Affiche : "Score existe !" car "0" est une chaîne non vide → truthy
```

```js
let x = 0;

if (x == false) {
  console.log("Égal avec coercition");
}

if (x === false) {
  console.log("Égalité stricte"); // ne sera pas exécuté
}
```



## 📌 Résumé

| Concept              | Exemple                                        | Résultat ou explication           |
| -------------------- | ---------------------------------------------- | --------------------------------- |
| Coercition implicite | `123 + "abc"`                                  | `"123abc"` → `123` devient chaîne |
| `==`                 | `"1" == 1`                                     | `true` avec conversion            |
| `===`                | `"1" === 1`                                    | `false`, types différents         |
| Booléen implicite    | `if([])`                                       | `true`, tableau vide est truthy   |
| Falsy                | `""`, `0`, `null`, `undefined`, `NaN`, `false` | Sont évalués à `false`            |

## 📚 Liens utiles

- [Type conversion - Glossary | MDN](https://developer.mozilla.org/en-US/docs/Glossary/Type_Conversion)

- [Equality comparisons and sameness - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Equality_comparisons_and_sameness)

- [Truthy - Glossary | MDN](https://developer.mozilla.org/en-US/docs/Glossary/Truthy)

- [Falsy - Glossary | MDN](https://developer.mozilla.org/en-US/docs/Glossary/Falsy)

- [if…else - JavaScript | MDN](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Statements/if...else)


