# 🌐 Le DOM et le BOM en JavaScript

## 📘 Qu’est-ce que le DOM ?

**DOM** = *Document Object Model*

C’est une **représentation en mémoire du code HTML** d’une page web.  
Il permet à JavaScript d’interagir dynamiquement avec la page.

> 👉 C’est une **interface de programmation** (API) fournie par le navigateur pour lire et manipuler le document HTML.

### 

### 🧠 Comment ça marche ?

- Le navigateur **parse** le HTML → crée une **structure arborescente** (nœuds, éléments, attributs, textes).

- Chaque élément HTML devient un **objet JavaScript** manipulable via `document`.



### 🎯 À quoi ça sert ?

- Ajouter, supprimer ou modifier des éléments HTML

- Changer dynamiquement du texte, des styles, des attributs

- Gérer les événements utilisateur (clic, clavier…)

- Créer des interfaces réactives ou animées



## 🌳 Illustration du DOM

Voici un aperçu d'une **structure DOM typique** :

```css
document
└── html
    ├── head
    │   └── title
    └── body
        ├── h1
        ├── p
        └── ul
            ├── li
            └── li
```

Chaque balise devient un **nœud DOM** que tu peux manipuler.



## 🎯 Sélectionner des éléments dans le DOM

### ✅ Sélecteurs classiques

| Méthode                          | Description                   |
| -------------------------------- | ----------------------------- |
| `getElementById(id)`             | Sélectionne un élément par ID |
| `getElementsByClassName(classe)` | Par classe (HTMLCollection)   |
| `getElementsByTagName(tag)`      | Par balise (HTMLCollection)   |

### ✅ Sélecteurs modernes (CSS)

| Méthode                       | Description                               |
| ----------------------------- | ----------------------------------------- |
| `querySelector(sélecteur)`    | Retourne le **1er élément** CSS trouvé    |
| `querySelectorAll(sélecteur)` | Retourne **tous les éléments** (NodeList) |

**Exemples :**

```js
const titre = document.getElementById("monTitre");
const boutons = document.querySelectorAll(".btn");
```



## 💡 Astuces pratiques et débogage

🔍 Inspecter un élément

```js
console.dir(document.body); // inspecte l'objet DOM
console.log(document.body); // montre le HTML dans la console
```

📋 Consulter le contenu

```js
document.title         // Titre de la page
document.URL           // URL actuelle
document.links         // Tous les <a>
document.images        // Toutes les images
document.forms         // Tous les formulaires
```



## 🧰 Outils de débogage DOM

- 📎 **Inspecteur HTML** (clic droit → Inspecter)

- 🧪 `console.log()` pour vérifier les sélections

- 🧠 `console.dir()` pour explorer les propriétés JS

- 📏 `element.getBoundingClientRect()` pour les dimensions



# 🌐 Qu’est-ce que le BOM ?

**BOM** = *Browser Object Model*

Le **BOM est l’interface JavaScript du navigateur lui-même**, c’est-à-dire **tout ce qui entoure la page** : l’URL, l’historique, l’écran, les fenêtres, etc.



### 📦 Le BOM contient :

- `window` → racine de tout dans le navigateur

- `location` → URL actuelle

- `navigator` → infos sur le navigateur

- `screen` → résolution de l’écran

- `history` → historique de navigation



## 📂 Propriétés utiles du BOM

`window`

```js
console.log(window.innerWidth, window.innerHeight);
console.log(window.alert("Bienvenue !"));
```

➡️ Tous les objets du DOM sont des **enfants implicites de `window`**

`location`

```js
console.log(location.href);      // URL actuelle
location.reload();               // Recharger la page
location.assign("https://...");  // Aller vers une URL
```

``navigator``

```js
console.log(navigator.userAgent);     // Infos navigateur
console.log(navigator.language);      // Langue du navigateur
```

``screen``

```js
console.log(screen.width, screen.height); // Dimensions écran
```



**L’objet `history`**

Gère l’historique de navigation du navigateur :

```js
history.back();    // Page précédente
history.forward(); // Page suivante
history.length;    // Nb de pages dans l’historique
```



**Déboguer le BOM**

```js
console.log(window);           // Tout l’environnement
console.log(location);         // Objet location complet
console.log(navigator);        // Infos navigateur
```



## Résumé

| Élément | DOM                              | BOM                                       |
| ------- | -------------------------------- | ----------------------------------------- |
| Rôle    | Représente la **structure HTML** | Représente l’**environnement navigateur** |
| Accès   | `document`                       | `window`, `navigator`, `location`, etc.   |
| Utilité | Manipulation de la page          | Redirection, alertes, historique, etc.    |


