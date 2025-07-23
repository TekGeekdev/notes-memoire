# Les opérateurs de coupe-circuit (`&&`, `||`) en JavaScript

## 🧠 1. Qu’est-ce qu’un opérateur de coupe-circuit ?

Un **opérateur de coupe-circuit** est un **opérateur logique** (`&&` ou `||`) qui **interrompt l’évaluation d’une expression** dès que le résultat est connu.

> 🧪 Cela permet **d’optimiser le code** et de **réaliser des affectations ou des actions conditionnelles** très pratiques.



## 🧩 2. `&&` (ET logique) — *Coupe-circuit à gauche si faux*

```js
condition1 && condition2
```

- Si `condition1` est **falsy**, JavaScript **n’évalue pas `condition2`**.

- Il retourne immédiatement la **valeur falsy** de gauche.

**Exemple**

```js
let connecté = false;

connecté && console.log("Bienvenue !");
```

➡️ Ici, `connecté` est `false` → donc **`console.log` n’est jamais exécuté**.



### 💡 Cas utile : exécuter du code seulement si une condition est vraie

```js
let estAdmin = true;

estAdmin && afficherPanneauAdmin();

function afficherPanneauAdmin() {
  console.log("Panneau admin visible");
}
```



## 🔀 3. `||` (OU logique) — *Coupe-circuit à gauche si vrai*

```js
valeur1 || valeur2
```

- Si `valeur1` est **truthy**, JavaScript **n’évalue pas `valeur2`**.

- Il retourne immédiatement la **valeur truthy** de gauche.

**Exemple**

```js
let pseudo = "";

let nomAffiché = pseudo || "Utilisateur anonyme";

console.log(nomAffiché); // "Utilisateur anonyme"
```

➡️ Si `pseudo` est vide (falsy), on utilise la valeur de droite.



## ⚠️ 4. Ne retourne pas forcément un booléen

Les opérateurs `&&` et `||` **ne retournent pas `true` ou `false`**, mais **la dernière valeur évaluée** :

```js
console.log("a" && "b"); // "b"
console.log(null || "défaut"); // "défaut"
```



## 💥 5. Éviter les erreurs avec des falsy involontaires

```js
let age = 0;
let ageAffiché = age || "Inconnu";

console.log(ageAffiché); // "Inconnu" ❌
```

➡️ `0` est falsy, donc on obtient un comportement inattendu.

💡 Dans ce cas, on peut utiliser l’opérateur `??` (nullish coalescing) à la place. Ex. :

```js
let ageAffiché = age ?? "Inconnu";
```



## 🧠 6. Résumé visuel

| Expression                        | Résultat si...                         | Utilité principale                    |
| --------------------------------- | -------------------------------------- | ------------------------------------- |
| `A && B`                          | B si A est truthy, sinon A             | Exécuter B **si A est vrai**          |
| `A                                |                                        | B`                                    |
| `!A`                              | Inverse A                              | Négation logique                      |
| Ne retourne pas `true` ou `false` | Peut retourner `null`, `""`, `0`, etc. | Comprendre le **type exact** retourné |



## 📚 Liens utiles

[Opérateur de coalescence des nuls (Nullish coalescing operator) - JavaScript | MDN](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing)


