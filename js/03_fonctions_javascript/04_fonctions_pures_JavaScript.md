# Les fonctions pures en JavaScript

## 🧠 C’est quoi une fonction pure ?

Imagine une **machine à jus d’orange**.

Tu mets **2 oranges**, tu obtiens **1 verre de jus**.  
Tu remets **exactement 2 oranges**, tu obtiens **toujours le même jus**.

> 👉 **Une fonction pure, c’est une machine à jus fiable** :  
> Elle donne **toujours le même résultat**, avec les **mêmes ingrédients**.  
> Et elle **ne salit pas la cuisine** (elle ne modifie rien autour d’elle).

👉 Elle ne modifie rien à l’extérieur d’elle-même (pas de modification de variables globales, pas d’accès à l’interface, pas de console.log, etc.).



## ✅ Règle n°1 : Même entrée → même sortie

```js
function doubler(x) {
  return x * 2;
}
```

📌 Tu donnes `5`, tu obtiens `10`.  
📌 Tu redonnes `5`, tu obtiens encore `10`.  
👉 Pas de surprise = **pure** ✅



### ❌ Contre-exemple : résultat qui change selon une variable extérieure

```js
let taux = 2;

function doubler(x) {
  return x * taux;
}
```

😕 Si `taux` change (ex. passe à 3), le résultat change même si tu passes encore `5`.

📌 Tu ne contrôles plus tout dans la fonction : elle **dépend de l’extérieur** ❌  
👉 C’est une **fonction impure**.



## Règle n°2 : Aucune action "à l’extérieur"

Une fonction pure **ne fait que retourner un résultat**.  
Elle **ne modifie rien**, **n’écrit rien**, **n’affiche rien**.

### Pure ✅

```js
function addition(a, b) {
  return a + b;
}
```

### Impure ❌ (effet de bord)

```js
function additionAvecLog(a, b) {
  console.log(a + b); // elle affiche = effet de bord
}
```

## 

## 📸 Résumé en image mentale

| 🧪 Fonction pure         | 🧨 Fonction impure                    |
| ------------------------ | ------------------------------------- |
| Comme une calculatrice   | Comme une machine qui fait du bruit   |
| Même input = même output | Output peut changer selon le contexte |
| Pas d’effet secondaire   | Peut modifier ou utiliser l’extérieur |
| Facile à tester          | Difficile à prédire                   |



## 🧠 Pourquoi c’est utile ?

Les fonctions pures sont :

- ✔️ **Prévisibles** : tu sais toujours ce qu’elles vont faire.

- ✔️ **Faciles à tester** : pas besoin de simuler un contexte.

- ✔️ **Sans danger** : elles ne vont pas casser autre chose sans que tu le saches.

- ✔️ **Réutilisables** : elles ne dépendent de rien sauf de leurs paramètres.



### Exemple dans un vrai contexte

```js
function calculerTotal(prix, taxes) {
  return prix + (prix * taxes);
}
```

📌 Tu donnes `10` et `0.15` → tu obtiens toujours `11.5`  
✅ Pas de `console.log`, pas de variable extérieure : elle est **pure**.




