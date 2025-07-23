# 🎯 **Tips pratiques JavaScript**/Bonus débug

## 🟢 🌟 Tips pratiques avec les chaînes (`String`)

### 📝 Vérifier le type

```js
typeof maChaine
```

➡️ Retourne `"string"`



**🔢 Longueur de la chaîne**

```js
maChaine.length
```

➡️ Nombre de caractères



**🔤 Accéder à un caractère précis**

```js
maChaine[0]     // Premier caractère
maChaine.charAt(0) // Idem
```



**🔍 Vérifier si vide**

```js
maChaine.length === 0
// ou
maChaine === ""
```



**🔡 Mettre en minuscules ou majuscules**

```js
maChaine.toLowerCase()
maChaine.toUpperCase()
```



**✂️ Extraire une portion**

```js
maChaine.slice(0, 3) // Les 3 premiers caractères
maChaine.substring(0, 3) // Idem
```



**🔄 Remplacer une partie**

```js
maChaine.replace("ancien", "nouveau")
```

✅ Pour remplacer **toutes** les occurrences (avec regex) :

```js
maChaine.replace(/ancien/g, "nouveau")
```



**🧩 Découper en tableau**

```js
maChaine.split(" ") // Découpe sur les espaces
maChaine.split("")  // Découpe chaque caractère
```



**🏁 Vérifier si ça commence ou finit par quelque chose**

```js
maChaine.startsWith("Hello")
maChaine.endsWith("World")
```



**🔍 Chercher un texte**

```js
maChaine.includes("mot")
```



**Répéter la chaîne**

```js
"*".repeat(10) // "**********"
```



## 🟢 🌟 Tips généraux pratiques

**Loguer plusieurs infos en une fois**

```js
console.log("Variable:", variable, "Type:", typeof variable)
```

**Loguer sous forme de tableau ou objet**

```js
console.table(maChaine.split(""))
```

**Voir les propriétés disponibles**

```js
console.dir(maChaine)
```

**🕵️ Voir plus d’infos en inspectant dans le debugger**

Si tu mets un `debugger;` :

```js
debugger;
```

puis tu ouvres la console dev tools, tu peux :  
✅ Regarder `variable` en détail  
✅ Voir ses prototypes  
✅ Essayer des méthodes en direct



**Vérifier le constructeur**

```js
variable.constructor.name
```

Par exemple :

```js
"test".constructor.name // "String"
[].constructor.name     // "Array"
({}).constructor.name   // "Object"
```

## Petit script complet de debug prêt à copier-coller

**debug-string.js**

```js
/**
 * Fonction utilitaire pour inspecter rapidement une chaîne
 * @param {string} str
 */
function debugString(str) {
  console.log("🔍 Inspecting String:");
  console.log("-----------------------");
  console.log("Type:", typeof str);
  console.log("Constructor:", str.constructor.name);
  console.log("Length:", str.length);
  console.log("Is empty:", str.length === 0);
  console.log("First character:", str[0]);
  console.log("Last character:", str[str.length - 1]);
  console.log("Uppercase:", str.toUpperCase());
  console.log("Lowercase:", str.toLowerCase());
  console.log("Includes 'test'?", str.includes("test"));
  console.log("Starts with 'Hello'?", str.startsWith("Hello"));
  console.log("Ends with 'world'?", str.endsWith("world"));
  console.log("Split by space:", str.split(" "));
  console.table(str.split(""));
}

/**
 * Exemple d'utilisation
 */
const myString = "Hello world";
debugString(myString);

```

## 🟢 🌟 Tips pratiques avec les nombres (`Number`)

**Vérifier si pair/impair**

```js
num % 2 === 0 // pair
num % 2 !== 0 // impair
```

**Limiter à un nombre de décimales**

```js
Number(num.toFixed(3))
```

**Clamp entre min et max**

```js
Math.min(Math.max(num, min), max)
```

**Comparer avec une tolérance (pour les flottants)**

```js
Math.abs(a - b) < Number.EPSILON
```

### Petit script complet de debug prêt à copier-coller

**debug-number.js**

```js
/**
 * Fonction utilitaire pour inspecter rapidement un nombre
 * @param {number} num
 */
function debugNumber(num) {
  console.log("🔍 Inspecting Number:");
  console.log("-----------------------");
  console.log("Type:", typeof num);
  console.log("Constructor:", num.constructor.name);
  console.log("Is NaN:", isNaN(num));
  console.log("Is finite:", isFinite(num));
  console.log("Is integer:", Number.isInteger(num));
  console.log("Is safe integer:", Number.isSafeInteger(num));
  console.log("Is positive:", num > 0);
  console.log("Is negative:", num < 0);
  console.log("Absolute value:", Math.abs(num));
  console.log("Rounded:", Math.round(num));
  console.log("Fixed (2 decimals):", num.toFixed(2));
  console.log("Exponential:", num.toExponential());
  console.log("Binary representation:", num.toString(2));
  console.log("Hex representation:", num.toString(16));
}

/**
 * Exemple d'utilisation
 */
const myNumber = 42.567;
debugNumber(myNumber);
```

## 🟢 🌟 Tips pratiques avec les tableaux (`array`)

**Vérifier si c’est un tableau**

```js
Array.isArray(myArray)
```

**Créer un tableau rapidement**

```js
const arr = [1, 2, 3];
const arrEmpty = Array(5); // Tableau de 5 cases vides
const arrFilled = Array(5).fill(0); // [0,0,0,0,0]
```

**Copier un tableau**

```js
const copy = [...arr];
const clone = arr.slice();
```

**Obtenir le premier ou le dernier élément**

```js
arr[0];
arr.at(-1); // ES2022, dernier élément
```

**Trier**

```js
arr.sort(); // Tri alphabétique
arr.sort((a,b) => a - b); // Tri numérique croissant
```

**Boucler facilement**

```js
arr.forEach(el => console.log(el));
```

**Vérifier si ça contient une valeur**

```js
arr.includes(42);
```

**Vérifier si tous ou certains éléments remplissent une condition**

```js
arr.every(x => x > 0); // tous >0 ?
arr.some(x => x > 0);  // au moins un >0 ?
```

**Découper**

```js
arr.slice(1,3); // extrait indices 1 et 2
```

**Transformer un tableau**

```js
arr.map(x => x * 2);
```

**Filtrer**

```js
arr.filter(x => x !== null);
```

**Réduire à une seule valeur**

```js
arr.reduce((acc, val) => acc + val, 0);
```

**Fusionner**

```js
const merged = [...arr1, ...arr2];
```

**Supprimer le dernier ou premier élément**

```js
arr.pop(); // supprime dernier
arr.shift(); // supprime premier
```

### Petit script complet de debug prêt à copier-coller

**debug-array.js**

```js
/**
 * Fonction utilitaire pour inspecter rapidement un tableau
 * @param {Array} arr
 */
function debugArray(arr) {
  console.log("🔍 Inspecting Array:");
  console.log("-----------------------");
  console.log("Type:", typeof arr);
  console.log("Constructor:", arr.constructor.name);
  console.log("Is array:", Array.isArray(arr));
  console.log("Length:", arr.length);
  console.log("Is empty:", arr.length === 0);
  console.log("First element:", arr[0]);
  console.log("Last element:", arr[arr.length - 1]);
  console.log("Includes 'test'?", arr.includes("test"));
  console.log("Sorted copy:", [...arr].sort());
  console.log("Reversed copy:", [...arr].reverse());
  console.log("Joined (comma):", arr.join(", "));
  console.table(arr);
}

/**
 * Exemple d'utilisation
 */
const myArray = ["apple", "banana", "cherry"];
debugArray(myArray);

```

## 🟢 🌟 Tips pratiques avec les objets (`Objet`)

**Créer un objet**

```js
const obj = { name: "Alice", age: 30 };
```

**Vérifier qu’une propriété existe**

```js
obj.hasOwnProperty("name");
"name" in obj;
```

**Obtenir les clés, valeurs ou entrées**

```js
Object.keys(obj);   // ["name", "age"]
Object.values(obj); // ["Alice", 30]
Object.entries(obj); // [["name","Alice"], ["age",30]]
```

**Copier un objet**

```js
const clone = { ...obj };
```

**Fusionner des objets**

```js
const merged = { ...obj1, ...obj2 };
```

**Parcourir les paires clé/valeur**

```js
for (const [key, value] of Object.entries(obj)) {
  console.log(key, value);
}
```

**Supprimer une propriété**

```js
delete obj.age;
```

**Convertir un objet en JSON**

```js
JSON.stringify(obj);
```

Et l’inverse :

```js
JSON.parse('{"name":"Alice"}');
```

**Vérifier si l’objet est vide**

```js
Object.keys(obj).length === 0;
```

### Petit script complet de debug prêt à copier-coller

**debug-object.js**

```js
/**
 * Fonction utilitaire pour inspecter rapidement un objet
 * @param {Object} obj
 */
function debugObject(obj) {
  console.log("🔍 Inspecting Object:");
  console.log("-----------------------");
  console.log("Type:", typeof obj);
  console.log("Constructor:", obj.constructor?.name);
  console.log("Is null:", obj === null);
  console.log("Is plain object:", obj?.constructor?.name === "Object");
  console.log("Keys:", Object.keys(obj));
  console.log("Values:", Object.values(obj));
  console.log("Entries:", Object.entries(obj));
  console.log("Has property 'test':", obj.hasOwnProperty?.("test"));
  console.table(obj);
}

/**
 * Exemple d'utilisation
 */
const myObject = {
  name: "Alice",
  age: 30,
  isAdmin: true
};
debugObject(myObject);
```

## 🟢 🌟 Tips pratiques avec les booleans (`boolean`)

**Convertir en booléen**

```js
Boolean(val);
!!val;
```

**Négation**

```js
!val;
```

**Vérifier si une variable est vraie ou fausse**

```js
if (val) {
  console.log("C'est truthy");
} else {
  console.log("C'est falsy");
}
```

**Falsy en JavaScript**:

- `false`

- `0`

- `""` (chaîne vide)

- `null`

- `undefined`

- `NaN`

**Court-circuit logique**

```js
// Si val est truthy, retourne val, sinon retourne "default"
const result = val || "default";
```

**Ternaires**

```js
const status = isActive ? "actif" : "inactif";
```

**Inverser un booléen**

```js
isActive = !isActive;
```

**Sélectionner la dernière valeur si aucune n’est truthy**

```js
const final = a || b || c || "fallback";
```

**Sélectionner la première valeur définie**

```js
const final = a ?? "default"; // nullish coalescing
```

### Petit script complet de debug prêt à copier-coller

**debug-boolean.js**

```js
/**
 * Fonction utilitaire pour inspecter rapidement un booléen
 * @param {boolean} bool
 */
function debugBoolean(bool) {
  console.log("🔍 Inspecting Boolean:");
  console.log("-----------------------");
  console.log("Type:", typeof bool);
  console.log("Constructor:", bool.constructor.name);
  console.log("Is true:", bool === true);
  console.log("Is false:", bool === false);
  console.log("Negation:", !bool);
  console.log("Double negation (truthy?):", !!bool);
  console.log("As number:", Number(bool));
}

/**
 * Exemple d'utilisation
 */
const myBool = true;
debugBoolean(myBool);
```


