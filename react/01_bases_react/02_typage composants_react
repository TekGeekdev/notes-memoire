# Typage simple des composants React (TSX)

Une version courte et claire pour bien débuter avec **React + TypeScript**.

---

## 1) Composant basique

```tsx
// Hello.tsx

type HelloProps = {
  name: string;
};

export default function Hello({ name }: HelloProps) {
  return <h1>Hello {name} 👋</h1>;
}
```

👉 Ici :

- `HelloProps` définit les **props** du composant.
- Le composant reçoit `name` (string) et l’affiche.
- `export default` permet d’importer facilement :
  ```tsx
  import Hello from "./Hello";
  ```

---

## 2) Props optionnelles et valeurs par défaut

```tsx
type ButtonProps = {
  text: string;
  disabled?: boolean; // optionnel
};

export function Button({ text, disabled = false }: ButtonProps) {
  return <button disabled={disabled}>{text}</button>;
}
```

👉 Ici : `disabled` est optionnel (`?`).

---

## 3) `children`

```tsx
type CardProps = {
  title: string;
  children: React.ReactNode;
};

export function Card({ title, children }: CardProps) {
  return (
    <div className="p-4 border rounded">
      <h2>{title}</h2>
      <div>{children}</div>
    </div>
  );
}
```

👉 Permet de passer du contenu **entre les balises** :

```tsx
<Card title="Profil">
  <p>Contenu interne</p>
</Card>
```

---

## 4) Événements typés

```tsx
export function Input() {
  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    console.log(e.target.value);
  };

  return <input onChange={handleChange} />;
}
```

👉 Les événements ont toujours un **type précis** :

- `ChangeEvent<HTMLInputElement>` pour un input.
- `MouseEvent<HTMLButtonElement>` pour un bouton.

---

## 5) Étendre les props HTML

```tsx
type LinkProps = React.ComponentPropsWithoutRef<"a"> & {
  highlight?: boolean;
};

export function Link({ highlight, ...props }: LinkProps) {
  return <a {...props} className={highlight ? "text-blue-600" : ""} />;
}
```

👉 Ici, le composant accepte toutes les props d’un `<a>` classique.

---

## 6) Doc rapide (JSDoc)

```tsx
/**
 * Bouton réutilisable.
 * @param text - Texte affiché dans le bouton
 * @param disabled - Rend le bouton inactif (facultatif)
 */
type ButtonProps = {
  text: string;
  disabled?: boolean;
};
```

👉 Cette doc apparaît dans l’IDE (VSCode, WebStorm).

---

## 7) Différence entre `type` et `interface`

- **`type`** : sert à créer un alias (peut être primitif, union, intersection, tuple, etc.).
  ```ts
  type Status = "success" | "error";
  ```
- **`interface`** : sert surtout pour définir la forme d’un objet, peut être étendue ou fusionnée.
  ```ts
  interface User {
    id: number;
    name: string;
  }
  interface User {
    email?: string;
  } // fusion automatique
  ```
  👉 En pratique :
- Utilise **interface** quand tu décris un objet ou une classe.
- Utilise **type** pour des unions (`A | B`), des tuples, ou pour combiner plusieurs types.

---

## Sources utiles

- [React TypeScript Cheatsheets](https://react-typescript-cheatsheet.netlify.app/docs/basic/getting-started/introduction/)
- [TypeScript Handbook – Interfaces](https://www.typescriptlang.org/docs/handbook/interfaces.html)
- [TypeScript Handbook – Advanced Types](https://www.typescriptlang.org/docs/handbook/2/types-from-types.html)

---

## Résumé

- Définir un `type` ou `interface` pour les **props**.
- Utiliser `?` pour rendre une prop **optionnelle**.
- `children` = contenu entre les balises.
- Bien typer les **événements** (`ChangeEvent`, `MouseEvent`).
- `ComponentPropsWithoutRef<'tag'>` pour réutiliser les props HTML.
- Documenter avec des **commentaires JSDoc**.
- `type` = plus flexible, `interface` = pour objets extensibles.
