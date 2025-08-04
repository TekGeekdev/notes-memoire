# Les fonctions d’ordre supérieur en JavaScript

## 🧠 Définition simple

Une **fonction d’ordre supérieur** est une fonction qui :

1. **Prend une ou plusieurs fonctions en argument**  
   *OU*

2. **Retourne une fonction**

> 👉 Autrement dit : une fonction qui travaille avec **d’autres fonctions**.



## 🧃 Métaphore simple

Imagine que tu fais une recette.  
Tu as un **robot de cuisine** (la fonction d’ordre supérieur) à qui tu donnes :

- des **ingrédients** (valeurs),

- une **instruction spéciale** (fonction à exécuter sur chaque ingrédient).

Le robot **applique la fonction** à chaque ingrédient.



### Exemple 1 — Une fonction qui prend une fonction en argument

```js
function saluer(nom) {
  return "Bonjour " + nom;
}

function executer(fn, valeur) {
  return fn(valeur);
}

executer(saluer, "Alice"); // "Bonjour Alice"
```

👉 Ici, `executer` est une **fonction d’ordre supérieur** car elle **prend une autre fonction** en argument (`saluer`).

### Exemple 2 — Une fonction qui retourne une fonction

```js
function multiplier(facteur) {
  return function(valeur) {
    return valeur * facteur;
  };
}

const doubler = multiplier(2);
doubler(5); // 10
```

👉 `multiplier` est une fonction d’ordre supérieur car elle **retourne une fonction**.



## 🧰 Très utilisé avec les fonctions de tableau

Les fonctions suivantes sont **des fonctions d’ordre supérieur** :

- `.map()`

- `.filter()`

- `.reduce()`

- `.forEach()`

**Exemple avec `.map()`**

```js
const nombres = [1, 2, 3];

const carrés = nombres.map(function(nombre) {
  return nombre * nombre;
});

console.log(carrés); // [1, 4, 9]
```

👉 `map()` prend une **fonction en argument** → donc c’est une fonction d’ordre supérieur



## 🧠 Pourquoi c’est puissant ?

- ✔️ Permet d’écrire du code plus **réutilisable** et **modulaire**

- ✔️ Rend le code **plus expressif** (plus proche du langage humain)

- ✔️ Facilite la programmation **fonctionnelle**

- ✔️ Permet de **composer des comportements dynamiques**



## 💡 Astuce

Une fonction d’ordre supérieur **ne fait rien seule** :  
elle **oriente le comportement** selon **la fonction qu’on lui passe**.

C’est comme dire à quelqu’un :

> "Je te donne la liste, fais ce que tu veux avec chaque élément."



## 📚 Liens utiles

- [Fonctions et portée des fonctions - JavaScript | MDN](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Functions)

- [Array.prototype.map() - JavaScript | MDN](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

- [Array.prototype.filter() - JavaScript | MDN](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)

- [Array.prototype.reduce() - JavaScript | MDN](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)

- [MDN - forEach()](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)


