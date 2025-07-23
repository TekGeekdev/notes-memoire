# 🛠️ Outils de Débogage en JavaScript

## 🐞 1. `console.log()`

La méthode la plus simple pour inspecter les valeurs.

### 🎯 Exemple

```js
let x = 42;
console.log("La valeur de x est:", x);
```

✅ Peut afficher :

- Variables simples

- Objets

- Tableaux

## 📋 2. Autres méthodes `console`

| Méthode                                | Utilité                                  |
| -------------------------------------- | ---------------------------------------- |
| `console.log()`                        | Afficher des informations générales      |
| `console.error()`                      | Afficher des messages d’erreur           |
| `console.warn()`                       | Afficher des avertissements              |
| `console.table()`                      | Afficher des tableaux ou objets en table |
| `console.dir()`                        | Afficher une vue détaillée d’un objet    |
| `console.time()` / `console.timeEnd()` | Mesurer la durée d’exécution             |

```js
const users = [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" }
];
console.table(users);
```

## 🛑 3. Le mot-clé `debugger`

Permet de **mettre un point d’arrêt dans le code**, qui stoppe l’exécution si la console de développement est ouverte.

### 🎯 Exemple

```js
function calculerTotal(a, b) {
  debugger; // L'exécution s'arrête ici
  return a + b;
}
```

## 🌐 4. Les Outils de Développement du Navigateur

Tous les navigateurs modernes proposent des outils intégrés :

- **Google Chrome / Chromium**

- **Firefox**

- **Edge**

- **Safari**

### Fonctions principales :

✅ Inspecter le DOM  
✅ Visualiser les scripts et les sources  
✅ Mettre des points d’arrêt  
✅ Suivre la pile d’appels (*Call Stack*)  
✅ Surveiller les variables locales  
✅ Profiler les performances

## 🧭 5. Breakpoints (Points d’arrêt)

Pour arrêter l’exécution sur une ligne spécifique :

1. Ouvre l’onglet **Sources** (Chrome) ou **Debugger** (Firefox).

2. Clique sur le numéro de ligne souhaité.

3. Recharge ou exécute le code.

4. Le programme s’interrompt à cet endroit.

## 🎯 6. Surveillance des Variables

Pendant un point d’arrêt, tu peux :

- Passer la souris sur une variable pour voir sa valeur.

- Utiliser le panneau **Scope** pour consulter toutes les variables locales.

- Modifier les valeurs en direct.

- Exécuter des expressions dans la **Console**.

## ⏳ 7. Step by Step (Pas à Pas)

Quand l’exécution est interrompue, tu peux :

| Action            | Icône (Chrome)                | Utilité                          |
| ----------------- | ----------------------------- | -------------------------------- |
| **Step Over**     | ⬇️ (flèche courbée)           | Exécuter la ligne et avancer     |
| **Step Into**     | ⬇️➡️ (flèche pointant dedans) | Entrer dans une fonction appelée |
| **Step Out**      | ⬆️ (flèche vers le haut)      | Sortir de la fonction courante   |
| **Resume Script** | ▶️                            | Reprendre l’exécution normale    |

## 📝 Résumé des Bonnes Pratiques

✅ Utilise `console.log()` pour un débogage rapide  
✅ Place `debugger` dans le code si besoin de stopper l’exécution  
✅ Apprends à utiliser les outils du navigateur (Sources, Breakpoints, Call Stack)  
✅ Pense à retirer les logs et `debugger` avant de mettre en production

---

## 📚 Liens Utiles

- [MDN - console](https://developer.mozilla.org/fr/docs/Web/API/Console)

- [MDN - debugger](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Instructions/debugger)

- [Chrome for Developers](https://developer.chrome.com/docs/devtools?hl=fr)

- [Firefox Debugger Guide](https://developer.mozilla.org/en-US/docs/Tools/Debugger)


