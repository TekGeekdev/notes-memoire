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

## ✅ Nommage des Booléens

Quand une variable est un **booléen**, privilégier des préfixes explicites :

| Préfixe  | Exemple                        |
| -------- | ------------------------------ |
| `is`     | `isActive`, `isEnabled`        |
| `has`    | `hasChildren`, `hasPermission` |
| `can`    | `canEdit`, `canDelete`         |
| `should` | `shouldRetry`, `shouldSave`    |

**Exemples :**

```js
let isVisible = true;
let hasError = false;
let canSave = true;
let shouldRender = false;
```

## ✅ Nommage des Tableaux

Pour les **tableaux**, privilégier :

- Noms **pluriels**

- Ou suffixes `List`, `Array`, `Items`

**Exemples :**

```js
let users = ["Alice", "Bob"];
let errorMessages = [];
let productIds = [1, 2, 3];
let selectedItems = [];
```

## ✅ Nommage des Objets

Pour les **objets**, privilégier :

- Noms **singuliers**

- Eventuellement suffixes `Data`, `Info`, `Config`

**Exemples :**

```js
let user = { name: "Alice", age: 30 };
let config = { darkMode: true };
let orderInfo = { id: 123, total: 49.99 };
```

## 🧪 3. Vérifier le Type

Pour connaître le type d’une variable, utiliser `typeof` :

```js
let x = "Hello";
console.log(typeof x); // "string"

x = 42;
console.log(typeof x); // "number"
```

## 🧩 Tableau récapitulatif des conventions de nommage de fonctions

| Préfixe / Modèle | Description                                | Exemple d’utilisation                 |
| ---------------- | ------------------------------------------ | ------------------------------------- |
| `get...()`       | Retourne une valeur                        | `getUserName()`, `getWeather()`       |
| `set...()`       | Affecte ou met à jour une valeur           | `setUserAge()`, `setTheme()`          |
| `is...()`        | Retourne un booléen                        | `isAuthenticated()`, `isVisible()`    |
| `has...()`       | Vérifie une propriété                      | `hasAccess()`, `hasPermission()`      |
| `can...()`       | Vérifie une capacité ou un droit           | `canEdit()`, `canSave()`              |
| `should...()`    | Indique une condition logique              | `shouldRender()`, `shouldRetry()`     |
| `check...()`     | Vérifie une condition (similaire à `is`)   | `checkPassword()`, `checkInput()`     |
| `create...()`    | Crée un nouvel élément ou objet            | `createUser()`, `createListElement()` |
| `build...()`     | Construit une structure complexe           | `buildQuery()`, `buildFormData()`     |
| `generate...()`  | Produit quelque chose de nouveau           | `generateToken()`, `generateReport()` |
| `calc...()`      | Effectue un calcul et retourne le résultat | `calcTotal()`, `calcAverage()`        |
| `update...()`    | Met à jour une valeur existante            | `updateProfile()`, `updateStatus()`   |
| `remove...()`    | Supprime un élément                        | `removeItem()`, `removeUser()`        |
| `delete...()`    | Supprime (souvent en base de données)      | `deleteRecord()`, `deleteFile()`      |
| `fetch...()`     | Récupère des données à distance            | `fetchUsers()`, `fetchPosts()`        |
| `to...()`        | Convertit un type vers un autre            | `toString()`, `toJSON()`, `toEUR()`   |
| `handle...()`    | Gère un événement ou une action            | `handleClick()`, `handleSubmit()`     |
| `render...()`    | Affiche ou renvoie un élément visuel       | `renderPage()`, `renderButton()`      |
| `load...()`      | Charge des ressources ou données           | `loadData()`, `loadImage()`           |
| `save...()`      | Enregistre des données                     | `saveFile()`, `saveSettings()`        |

💡 Ces conventions sont inspirées des bonnes pratiques JavaScript modernes, notamment celles recommandées par **MDN**, **Airbnb JavaScript Style Guide** et **Clean Code (Robert C. Martin)**.



#### 📝 Résumé

✅ JavaScript est **dynamiquement typé**  
✅ Les variables changent de type au cours du programme  
✅ Respecter les conventions de nommage rend le code **prévisible et clair**  
✅ Utiliser des **préfixes cohérents** selon le type de variable ou de fonction  
✅ Utiliser `typeof` pour vérifier le type d’une variable



### 📚 Liens Utiles

- [MDN - Types](https://developer.mozilla.org/fr/docs/Web/JavaScript/Data_structures)

- [MDN - typeof](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Operators/typeof)

- [JavaScript.info - Variables](https://fr.javascript.info/variableshttps://fr.javascript.info/variables)

- [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)

- [Clean Code JavaScript (GitHub)](https://github.com/ryanmcdermott/clean-code-javascript)](https://developer.mozilla.org/fr/docs/Glossaire/Identificateur)
