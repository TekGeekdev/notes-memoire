# Les fonctions récursives en JavaScript

## 🧠 Définition simple

Une **fonction récursive** est une fonction qui **s'appelle elle-même** pour résoudre un problème.

> 👉 C’est comme une **poupée russe** : chaque poupée contient une version plus petite d’elle-même, jusqu’à la dernière.



## 🔁 Structure d'une fonction récursive

Elle a **toujours 2 parties essentielles** :

1. 🔚 Une **condition d’arrêt** pour ne pas tourner à l’infini

2. 🔁 Un **appel récursif** à la fonction elle-même



## ✅ Exemple simple — Compter à rebours

```js
function compteRebours(n) {
  if (n <= 0) {
    console.log("Terminé !");
    return;
  }
  console.log(n);
  compteRebours(n - 1); // appel récursif
}

compteRebours(3);
```

**Résultat :**

```js
3
2
1
Terminé !
```

✅ La fonction s’arrête quand `n <= 0` → **condition d'arrêt**



## 🔄 Ce qu’il se passe dans le cerveau de JS

Imagine une pile d’assiettes :

1. `compteRebours(3)` attend `compteRebours(2)`

2. `compteRebours(2)` attend `compteRebours(1)`

3. `compteRebours(1)` attend `compteRebours(0)`

4. `compteRebours(0)` affiche "Terminé !" et ne rappelle plus rien  
   ➡️ Chaque fonction empilée **se désempile en sens inverse**



## Exemple classique — Factorielle

La **factorielle** de 5 (notée `5!`) = 5 × 4 × 3 × 2 × 1

```js
function factorielle(n) {
  if (n === 0) return 1;           // condition d'arrêt
  return n * factorielle(n - 1);   // appel récursif
}

console.log(factorielle(5)); // 120
```



## ⚠️ Attention

- **Toujours prévoir une condition d’arrêt**

- Sinon tu auras une **erreur de pile maximale** :

```js
Uncaught RangeError: Maximum call stack size exceeded
```



## 📦 Pourquoi utiliser la récursivité ?

| Cas d’usage typiques                   | Exemples                               |
| -------------------------------------- | -------------------------------------- |
| Problèmes divisibles en sous-problèmes | Factorielle, Fibonacci, puissance      |
| Structures imbriquées                  | Parcourir un arbre, des dossiers, JSON |
| Alternatives aux boucles               | Quand une boucle serait trop complexe  |



## 🔁 Comparaison boucle vs récursion

**Avec une boucle :**

```js
for (let i = 3; i > 0; i--) {
  console.log(i);
}
```

**Avec récursion :**

```js
function countdown(i) {
  if (i === 0) return;
  console.log(i);
  countdown(i - 1);
}
```

📌 Choisir la récursivité :  
✔️ Quand la **structure du problème s'y prête** (arbre, division…)  
✖️ Sinon, une **boucle est plus simple et performante**



## 📚 Liens utiles

- [Récursion - Glossaire MDN : définitions des termes du Web | MDN](https://developer.mozilla.org/fr/docs/Glossary/Recursion)

- [Pile execution - Glossaire MDN : définitions des termes du Web | MDN](https://developer.mozilla.org/fr/docs/Glossary/Call_stack)


