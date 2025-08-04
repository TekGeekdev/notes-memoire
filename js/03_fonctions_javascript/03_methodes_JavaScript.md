# Les méthodes en JavaScript

## 🧠 Définition simple

En JavaScript, **une méthode est une fonction qui appartient à un objet**.

Autrement dit, c’est **une action que peut faire un objet**.

> 🟰 Une méthode, c’est une **fonction liée à un objet**.

## Syntaxe de base

```js
const voiture = {
  demarrer: function () {
    console.log("La voiture démarre");
  }
};

voiture.demarrer(); // Appel de la méthode
```

**Exemple concret**

```js
const utilisateur = {
  nom: "Alice",
  saluer: function () {
    console.log("Bonjour, je m'appelle " + this.nom);
  }
};

utilisateur.saluer(); // Bonjour, je m'appelle Alice
```

🔎 `this.nom` fait référence à la propriété `nom` de l’objet `utilisateur`.

## Méthodes raccourcies (ES6)

Depuis ES6, tu peux écrire les méthodes de manière plus courte :

```js
const utilisateur = {
  nom: "Bob",
  saluer() {
    console.log("Salut, je suis " + this.nom);
  }
};

utilisateur.saluer(); // Salut, je suis Bob
```

## 👀 Différence entre fonction et méthode

| Fonction                    | Méthode                                  |
| --------------------------- | ---------------------------------------- |
| Définie seule, hors objet   | Définie à l’intérieur d’un objet         |
| `function direBonjour() {}` | `utilisateur.saluer = function () {}`    |
| Appelée directement         | Appelée via un objet : `objet.methode()` |

## 🔄 Méthodes natives de JS

Certaines méthodes sont déjà intégrées dans JavaScript.

**Sur les chaînes de caractères**

```js
let phrase = "Bonjour tout le monde";

console.log(phrase.toUpperCase()); // "BONJOUR TOUT LE MONDE"
console.log(phrase.includes("monde")); // true
```

**Sur les tableaux**

```js
let nombres = [1, 2, 3];

console.log(nombres.length); // 3
nombres.push(4); // ajoute 4 à la fin
console.log(nombres); // [1, 2, 3, 4]
```

## 🧠 À retenir

- Une **méthode est une fonction attachée à un objet**.

- Tu l’appelles avec `objet.nomDeLaMéthode()`.

- Tu peux créer tes propres méthodes ou utiliser celles fournies par JS (sur les strings, arrays, etc.).

- Tu peux utiliser `this` dans une méthode pour parler de l’objet lui-même.



## 📚 Liens utiles

- [Prototypes Objet - Apprendre le développement web | MDN](https://developer.mozilla.org/fr/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_prototypes)


