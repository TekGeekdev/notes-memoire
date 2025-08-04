# Les fonctions auto-invoquées (IIFE) en JavaScript

## 🧠 Définition simple

Une **fonction auto-invoquée**, appelée aussi **IIFE** (`Immediately Invoked Function Expression`), est une fonction **qui s’exécute immédiatement après avoir été définie**.

> 👉 Elle s’auto-démarre, comme un moteur qui s’allume tout seul dès qu’il est assemblé.



## 🔧 Syntaxe de base

```js
(function () {
  console.log("Je suis une IIFE !");
})();
```

🔍 Ce qu’il se passe :

- `function () { ... }` → on définit une fonction **anonyme**

- `(function () { ... })` → on la **met entre parenthèses** pour la transformer en **expression**

- `()` → on ajoute des **parenthèses à la fin** pour **l’exécuter immédiatement**



## 💡 Pourquoi entre parenthèses ?

En JavaScript, une **function toute seule** est interprétée comme une **déclaration**, et on ne peut **pas l’exécuter directement**.

> 👉 En l’entourant de `()`, on dit :  
> *« Ceci n’est pas une déclaration, c’est une **expression**. »*



#### **Exemple simple**

```js
(function () {
  const secret = "mot de passe";
  console.log("Fonction exécutée immédiatement !");
})();
```

**🧪 Résultat :**

```js
Fonction exécutée immédiatement !
```

🔒 Le **secret** reste **inaccessible de l’extérieur** : c’est une **variable locale**.



#### Exemple avec des paramètres

```js
(function (nom) {
  console.log("Bonjour " + nom);
})("Math");
```

Comme une fonction normale, tu peux lui passer des arguments !



## À quoi ça sert ?

1. **Éviter les conflits de variables**

```js
(function () {
  const compteur = 0;
  // Cette variable n'existe qu'ici
})();
```

👉 `compteur` ne pollue pas le reste du code



2. **Créer un espace privé (scope isolé)**

```js
const app = (function () {
  const nom = "AppModule";
  return {
    getNom: function () {
      return nom;
    },
  };
})();

console.log(app.getNom()); // "AppModule"
```

👉 On simule un **module** avec des **données privées** 🔐



## 🧠 À retenir

| 🧩 Caractéristique                    | Détail                                                 |
| ------------------------------------- | ------------------------------------------------------ |
| IIFE = fonction + exécution immédiate | `(function () { ... })();`                             |
| Ne pollue pas le scope global         | Les variables à l’intérieur restent privées            |
| Très utilisé avant ES6                | Remplacé aujourd’hui par `let`, `const`, modules, etc. |


