# 🧱 Les objets en JavaScript



## 📖 Qu’est-ce qu’un objet ?

Un **objet** est une structure de données composée de **paires clé-valeur**.  
Chaque **clé** (appelée aussi *propriété*) est une chaîne (ou un symbole) associée à une **valeur** (qui peut être de n’importe quel type, y compris une fonction).



## 🛠️ Créer un objet

**Littéral d’objet**

```js
const personne = {
  nom: "Alice",
  age: 30,
  isAdmin: true
};
```

**Avec `new Object()`**

```js
const personne = new Object();
personne.nom = "Alice";
```

**Object.create()**

```js
const base = { isHuman: true };
const john = Object.create(base);
john.name = "John";
```



## 🧩 Accéder aux propriétés

**Par point**

```js
console.log(personne.nom); // "Alice"
```

**Par crochets (utile pour clés dynamiques ou avec des espaces)**

```js
console.log(personne["age"]); // 30
```



## ✏️ Modifier ou ajouter une propriété

```js
personne.age = 31;
personne.email = "alice@example.com";
```



## ❌ Supprimer une propriété

```js
delete personne.isAdmin;
```



## 🔍 Vérifier l’existence d’une propriété

```js
"nom" in personne           // true
personne.hasOwnProperty("nom") // true
```



## 📚 Parcourir un objet

**Avec `for...in`**

```js
for (let key in personne) {
  console.log(key, personne[key]);
}
```

**Avec `Object.keys()`, `Object.values()`, `Object.entries()`**

```js
Object.keys(personne);   // ["nom", "age"]
Object.values(personne); // ["Alice", 30]
Object.entries(personne); // [["nom", "Alice"], ["age", 30]]
```



## 🧠 Les fonctions dans les objets (méthodes)

```js
const user = {
  nom: "Bob",
  direBonjour: function() {
    console.log(`Bonjour, je m'appelle ${this.nom}`);
  }
};

user.direBonjour(); // "Bonjour, je m'appelle Bob"
```

❗`this` fait référence à l’objet lui-même.



## 🧊 Objets imbriqués

```js
const utilisateur = {
  nom: "Alice",
  adresse: {
    ville: "Paris",
    codePostal: "75000"
  }
};

console.log(utilisateur.adresse.ville); // "Paris"
```



## 🧩 Copier un objet

**⚠️ Copie **par référence** :**

```js
const a = { x: 1 };
const b = a;
b.x = 2;
console.log(a.x); // 2 ❗
```

C'est comme s'il faisait référence au a

**✅ Créer une **copie superficielle** :**

```js
const copie = { ...a };
```

Copie propre sans altération de a



## 🧪 Tester si un objet est vide

```js
Object.keys(obj).length === 0;
```



## Fusionner des objets

```js
const o1 = { a: 1 };
const o2 = { b: 2 };
const fusion = { ...o1, ...o2 }; // { a: 1, b: 2 }
```



## Décomposer un objet (destructuring)

```js
const voiture = { marque: "Toyota", annee: 2022 };
const { marque, annee } = voiture;
console.log(marque); // "Toyota"
```



## 🧠 Objet vs tableau

| Objet               | Tableau                 |
| ------------------- | ----------------------- |
| Clés personnalisées | Clés numériques (index) |
| Structure nommée    | Liste ordonnée          |
| `{ nom: "Alice" }`  | `["Alice", "Bob"]`      |

## 

## 📦 Objet global `Object`

JavaScript fournit l’objet natif `Object` avec des méthodes utilitaires :

| Méthode                     | Description                           |
| --------------------------- | ------------------------------------- |
| `Object.keys(obj)`          | Liste des clés                        |
| `Object.values(obj)`        | Liste des valeurs                     |
| `Object.entries(obj)`       | Tableau de [clé, valeur]              |
| `Object.assign(obj1, obj2)` | Copie/fusion d’objets                 |
| `Object.freeze(obj)`        | Rend l’objet immuable (lecture seule) |
| `Object.hasOwn(obj, "clé")` | Vérifie une clé propre (ES2022)       |



## 📝 Résumé rapide

| Action                         | Syntaxe ou méthode              |
| ------------------------------ | ------------------------------- |
| Créer un objet                 | `{ clé: valeur }`               |
| Accéder à une propriété        | `obj.clé` ou `obj["clé"]`       |
| Ajouter/modifier une propriété | `obj.clé = valeur`              |
| Supprimer une propriété        | `delete obj.clé`                |
| Parcourir les propriétés       | `for...in`, `Object.entries()`  |
| Copier un objet                | `{ ...obj }`                    |
| Fusionner des objets           | `{ ...obj1, ...obj2 }`          |
| Tester si vide                 | `Object.keys(obj).length === 0` |



## 📚 Liens utiles

- [MDN - Objets](https://developer.mozilla.org/fr/docs/Web/JavaScript/Guide/Working_with_Objects)

- [MDN - Object global](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Object)


