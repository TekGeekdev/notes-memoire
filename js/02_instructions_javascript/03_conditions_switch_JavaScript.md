# L’instruction `switch` en JavaScript

## 🧠 1. Qu’est-ce que `switch` ?

L’instruction `switch` permet de **tester la valeur d’une expression** contre **plusieurs cas possibles**.

C’est une **alternative plus lisible** à une série de `if...else if...else` lorsqu’on compare **une même variable** à plusieurs valeurs.



## 🧩 2. Syntaxe de base

```js
switch (expression) {
  case valeur1:
    // code exécuté si expression === valeur1
    break;
  case valeur2:
    // code exécuté si expression === valeur2
    break;
  default:
    // code exécuté si aucune des valeurs ne correspond
}
```

## 🔑 3. Rôle du mot-clé `break`

- **`break`** permet de sortir du `switch` après qu’un **case ait été exécuté**.

- Sans `break`, JavaScript continue d’exécuter les autres `case` même si la condition n’est plus vraie. C’est ce qu’on appelle le **"fallthrough"**.



**🧪 Exemple simple**

```js
let fruit = "pomme";

switch (fruit) {
  case "banane":
    console.log("C'est une banane !");
    break;
  case "pomme":
    console.log("C'est une pomme !");
    break;
  case "orange":
    console.log("C'est une orange !");
    break;
  default:
    console.log("Fruit inconnu");
}
```

👉 Résultat : `"C'est une pomme !"`



**⚠️ 4. Attention au **fallthrough****

```js
let note = "B";

switch (note) {
  case "A":
    console.log("Excellent !");
  case "B":
    console.log("Très bien !");
  case "C":
    console.log("Bien !");
  default:
    console.log("Résultat reçu.");
}
```

👉 Résultat :

```js
Très bien !
Bien !
Résultat reçu.
```

➡️ Ici, **pas de `break`**, donc tous les `case` suivants sont exécutés même s’ils ne correspondent pas.



## 🧠 5. Utilisation de `default`

- Le bloc `default` est **optionnel**, mais recommandé.

- Il s’exécute **si aucun `case` ne correspond**.

```js
let jour = "samedi";

switch (jour) {
  case "lundi":
  case "mardi":
  case "mercredi":
  case "jeudi":
  case "vendredi":
    console.log("C’est un jour de semaine");
    break;
  case "samedi":
  case "dimanche":
    console.log("C’est le week-end !");
    break;
  default:
    console.log("Jour inconnu");
}
```

## 

## 📌 Résumé

| Élément     | Description                                                 |
| ----------- | ----------------------------------------------------------- |
| `switch`    | Compare une valeur à plusieurs cas possibles                |
| `case`      | Bloc exécuté si la valeur correspond                        |
| `break`     | Interrompt l'exécution pour ne pas passer au cas suivant    |
| `default`   | Bloc par défaut si aucun `case` ne correspond               |
| Fallthrough | Quand on oublie `break` → tous les cas suivants s’exécutent |



## 📚 Liens utiles

[switch - JavaScript | MDN](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Statements/switch)

[break - JavaScript | MDN](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Statements/break)


