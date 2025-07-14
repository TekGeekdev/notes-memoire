# 🚀 Injection des scripts JavaScript et Types d’Exécution

## 📄 1. Injection des Scripts JavaScript

Il existe plusieurs façons d’ajouter du JavaScript dans une page HTML.

### ✅ 1.1 Script Interne

Le code JavaScript est directement écrit dans la page HTML, dans une balise `<script>`.

```html
<!DOCTYPE html>
<html>
<head>
  <title>Script interne</title>
</head>
<body>
  <h1>Hello World</h1>

  <script>
    console.log("Ceci est un script interne.");
  </script>
</body>
```

**Utilité :**

- Scripts simples

- Démonstrations

- Test rapide

### ✅ 1.2 Script Externe

Le code est placé dans un fichier `.js` séparé, puis importé.

**Fichier `script.js`**

```js
console.log("Ceci est un script externe.");
```

```html
<script src="script.js"></script>
```

**Avantages :**

- Réutilisation du code

- Meilleure organisation

- Mise en cache par le navigateur

### ✅ 1.3 Script Inline (événement)

Du JavaScript peut être écrit directement dans un attribut HTML, par exemple `onclick` :

```html
<button onclick="alert('Bouton cliqué !')">Clique-moi</button>
```

**Attention :**

- Moins recommandé

- Séparation du HTML et du JS moins claire

## ⚙️ 2. Emplacement des Scripts

L’emplacement de la balise `<script>` influence **le moment où le script est exécuté**.

🎯 2.1 Dans `<head>`

```html
<head>
  <script src="script.js"></script>
</head>
```

➡️ Le script est chargé et exécuté **avant le rendu du contenu HTML**.  
⚠️ Si le script accède au DOM, il peut provoquer des erreurs.

🎯 2.2 Avant `</body>`

```html
<body>
  <h1>Page prête</h1>
  <script src="script.js"></script>
</body>
```

✅ Le script est exécuté **après le chargement du contenu HTML**.  
👍 Recommandé si pas d’attribut `defer` ou `async`.

## ⏳ 3. Les Attributs `defer` et `async`

Ils permettent de contrôler **quand le script est téléchargé et exécuté**.

---

#### 🕒 3.1 bis Sans `defer` / `async`

- Le script est téléchargé et exécuté immédiatement.

- Le parsing du HTML est bloqué jusqu’à la fin du script.

### 🚦 3.1 `defer`

```js
<script src="script.js" defer></script>
```

✅ Téléchargement en parallèle  
✅ Exécution **après parsing du HTML**  
✅ Ordre garanti

### ⚡ 3.2 `async`

```js
<script src="script.js" async></script>
```

✅ Téléchargement en parallèle  
✅ Exécution dès disponibilité  
❌ Ordre non garanti

## 📦 4. Scripts de Type Module (`type="module"`)

Depuis ES6, JavaScript supporte les **modules natifs**, avec import/export.

✨ Exemple 

interne

```html
<script type="module">
  import { maFonction } from './utils.js';
  maFonction();
</script>
```

Ou externe :

```html
<script type="module" src="main.js"></script>
```

### 🎯 Caractéristiques des Modules

✅ Les scripts sont **toujours exécutés en mode strict** (`"use strict"`)  
✅ Les imports/exports sont disponibles  
✅ **Chargement différé** par défaut (comportement similaire à `defer`)  
✅ Scope local (les variables ne polluent pas `window`)  
✅ Nécessite un serveur HTTP (les modules ne fonctionnent pas avec `file://` dans certains navigateurs)

### 🌐 Exemple d’import dans un module

**fichier `utils.js`**

```js
export function maFonction() {
  console.log("Hello depuis utils.js !");
}
```

fichier `main.js`

```js
import { maFonction } from './utils.js';
maFonction();
```

## 📊 5. Comparatif des Types de Scripts

| Type            | Téléchargement  | Exécution             | Ordre garanti | Scope     |
| --------------- | --------------- | --------------------- | ------------- | --------- |
| *(rien)*        | Pendant parsing | Bloque parsing        | Oui           | Global    |
| `defer`         | En parallèle    | Après parsing complet | Oui           | Global    |
| `async`         | En parallèle    | Dès disponibilité     | Non           | Global    |
| `type="module"` | En parallèle    | Après parsing complet | Oui           | **Local** |

## 📝 Résumé des Bonnes Pratiques

✅ Utiliser **modules (`type="module")** pour un code moderne et structuré ✅ Privilégier` defer`pour les scripts classiques si pas de modules ✅ Éviter`async`sauf si ordre non critique ✅ Placer les scripts avant`</body>` si pas d’attributs  
✅ Toujours séparer HTML et JS autant que possible

## 📚 Liens Utiles

- [MDN - `<script>`](https://developer.mozilla.org/fr/docs/Web/HTML/Element/script)

- [MDN - Modules JavaScript](https://developer.mozilla.org/fr/docs/Web/JavaScript/Guide/Modules)

- [MDN - defer](https://developer.mozilla.org/fr/docs/Web/HTML/Element/script#attr-defer)

- [MDN - async](https://developer.mozilla.org/fr/docs/Web/HTML/Element/script#attr-async)




# Précisions

## 🟢 **1️⃣ Est-ce que `type="module"` peut être interne ?**

Oui :  
Quand tu écris :

```html
<script type="module">
  import { maFonction } from './utils.js';
  maFonction();
</script>
```

👉 c’est un **script interne** qui est **un module**.  
La différence avec un `<script>` classique :

- Tu peux faire des `import` et `export`.

- Le code s’exécute en **mode strict**.

- Il a son propre **scope local**.

Mais dans la pratique, on l’utilise rarement en interne :  
✅ C’est plus courant de mettre `type="module"` sur un script **externe** comme ça :

```html
<script type="module" src="main.js"></script>
```

ou de faire :

```js
// main.js
import { maFonction } from './utils.js';
maFonction();
```

## 🟢 **2️⃣ Est-ce que `type="module"` est surtout utilisé avec React ou Vue ?**

C'est vrai:  
✅ Les **frameworks modernes** (React, Vue, Svelte, etc.) utilisent presque toujours des modules ES (ou un bundler qui compile en modules).  
✅ Les modules ES permettent d’écrire du code **modulaire**, avec des `import/export` comme en Node.js.

Dans React ou Vue, c’est souvent caché derrière un bundler (Webpack, Vite, Parcel), qui transforme ton code `import ...` en gros fichier optimisé.

**Mais ça ne veut pas dire que tu ne peux pas l’utiliser sur un petit projet perso.**

✅ **Sur un projet simple**, par exemple un script qui a besoin de bien structurer ses fichiers, tu peux très bien utiliser `type="module"` sans React ni Vue.  
**Exemple :**

- `index.html`

- `main.js`

- `lib.js`

**lib.js**

```js
export function greet(name) {
  console.log(`Hello, ${name}!`);
}
```

**main.js**

```js
import { greet } from './lib.js';
greet("Mathieu");
```

**index.html**

```html
<script type="module" src="main.js"></script>
```

✅ Aucun framework, juste du JavaScript moderne.



## 🟢 **3️⃣ Ça vaut le coup de l’utiliser pour des petits scripts ?**

👉 Ça dépend :

### Quand ça vaut le coup

✅ Si tu veux découper ton code proprement en plusieurs fichiers avec `import/export`  
✅ Si tu veux bénéficier du mode strict par défaut  
✅ Si tu veux des dépendances plus claires  
✅ Si tu veux écrire du code moderne et apprendre les modules

### Quand ce n’est pas utile

❌ Si tu n’as qu’un seul fichier avec 10 lignes de code  
❌ Si tu n’as pas besoin d’importer/exporter  
❌ Si tu cherches la compatibilité maximale avec des navigateurs anciens

## 🟢 **4️⃣ Qu’est-ce que tu entends par :**

**"Nécessite un serveur HTTP (les modules ne fonctionnent pas avec file:// dans certains navigateurs)"**

👉 Les modules ES ont une restriction de sécurité :  
**Quand tu ouvres une page en double-cliquant sur un fichier HTML (`file://...`)**, certains navigateurs bloquent les imports relatifs (`import ... from './module.js';`).

✅ Ça fonctionne si tu ouvres via **un serveur HTTP**.

**Exemple :**

- Ça ne marche pas avec :

```perl
file:///C:/mon-projet/index.html
```

- Ça marche avec :

```bash
http://localhost:3000/index.html
```

**Comment lancer un petit serveur local ?**  
Hyper simple :

🌐 **1. Avec Python****

```shell
# Dans le dossier du projet
python -m http.server 8000
```

Puis ouvrir :

```bash
http://localhost:8000
```

🌐 **2. Avec Node.js**
Si tu as `npm install -g serve` :

```nginx
serve .
```

🌐 **3. Avec PHP**

Très simple si tu as PHP installé (Windows, macOS, Linux)

```bash
php -S localhost:8000
```

Ouvre :

```bash
http://localhost:8000
```

✅ Tous ces serveurs servent simplement tes fichiers HTML/CSS/JS par **HTTP**, ce qui est obligatoire pour que `type="module"` fonctionne dans certains navigateurs.

## 🟢 Détail : Le code s’exécute en mode strict

Quand tu utilises :

```html
<script type="module">
  // du code ici
</script>
```

ou

```html
<script type="module" src="main.js"></script>
```

✅ **JavaScript active automatiquement le "strict mode".**

### 📝 Qu’est-ce que le strict mode ?

Le **mode strict** (`"use strict"`) est un mode plus rigoureux de JavaScript qui :

- Empêche certaines erreurs silencieuses.

- Interdit les variables globales implicites.

- Sécurise l’utilisation de `this`.

- Réserve certains mots-clés.

### 🎯 Exemple

**Sans strict mode :**

```js
x = 10; // Oups, variable globale implicite
console.log(x); // 10
```

**Avec strict mode (ou module) :**

```js
x = 10; // Erreur !
```

Quand tu utilises `type="module"`, c’est comme si tu avais écrit :

```js
"use strict";
```

en haut de ton fichier.

## 🟢 Détail : Il a son propre scope local

Dans un `<script>` classique, toutes les variables déclarées sans bloc particulier vont directement dans le **scope global**, c’est-à-dire l’objet `window`.

**Exemple classique (sans module) :**

```html
<script>
  var x = 42;
  console.log(window.x); // 42
</script>
```

✅ La variable `x` est attachée à `window`.

**Avec `type="module"` :**

```html
<script type="module">
  var y = 42;
  console.log(window.y); // undefined
</script>
```

✅ Ici, la variable `y` est **locale au module** et n’existe pas sur `window`.

🎯 **En résumé :**

- **Scope global classique :**
  
  - Pollution facile de `window`
  
  - Risque de collisions de variables

- **Scope de module :**
  
  - Les variables restent dans le module
  
  - Pas de pollution du global
  
  - Plus sûr et plus propre

## 🟢 Pourquoi c’est important ?

✅ Avec `type="module"`, tu peux avoir des scripts mieux isolés, plus organisés et moins sujets aux bugs de portée.

✅ Ça rapproche JavaScript du fonctionnement des modules Node.js, et des frameworks modernes comme React, Vue, etc.



## 🟢 **Résumé clair**

✅ `type="module"` permet d’utiliser `import` et `export`  
✅ Peut être interne ou externe  
✅ Charge les scripts en mode "défer" automatiquement (après parsing du HTML)  
✅ Scope local  
✅ Nécessite un serveur HTTP pour fonctionner correctement  
✅ Très utilisé avec React, Vue, etc., mais parfaitement utilisable pour des scripts modernes simples
