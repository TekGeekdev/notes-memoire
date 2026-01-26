# Typage simple des composants React (TSX)

Une version courte et claire pour bien débuter avec **React + TypeScript**.

---

## 1) Composant basique avec JSDoc

````tsx
// Hello.tsx

/**
 * Props du composant Hello
 * @property name - Nom de l'utilisateur à afficher
 */
type HelloProps = {
  name: string;
};

/**
 * Composant d'affichage d'un message de bienvenue.
 * @param props - Les propriétés du composant Hello
 * @returns Un élément JSX affichant un titre de bienvenue
 * @example
 * ```tsx
 * <Hello name="Alice" />
 * ```
 */
export default function Hello({ name }: HelloProps) {
  return <h1>Hello {name} 👋</h1>;
}
````

👉 Ici :

- `HelloProps` documenté avec JSDoc.
- La fonction `Hello` a aussi sa doc (`@param`, `@returns`, `@example`).

---

## 2) Props optionnelles et valeurs par défaut

```tsx
/**
 * Props pour le composant Button
 * @property text - Texte affiché dans le bouton
 * @property disabled - Rend le bouton inactif (optionnel)
 */
type ButtonProps = {
  text: string;
  disabled?: boolean; // optionnel
};

/**
 * Bouton réutilisable.
 * @param props - Texte et état du bouton
 * @returns Un bouton HTML stylisé
 */
export function Button({ text, disabled = false }: ButtonProps) {
  return <button disabled={disabled}>{text}</button>;
}
```

---

## 3) `children`

```tsx
/**
 * Props pour le composant Card
 * @property title - Titre affiché en haut de la carte
 * @property children - Contenu passé entre les balises <Card>…</Card>
 */
type CardProps = {
  title: string;
  children: React.ReactNode;
};

/**
 * Carte avec un titre et du contenu.
 * @param props - Titre et contenu de la carte
 */
export function Card({ title, children }: CardProps) {
  return (
    <div className="p-4 border rounded">
      <h2>{title}</h2>
      <div>{children}</div>
    </div>
  );
}
```

---

## 4) Événements typés

```tsx
/**
 * Input avec gestion de l'événement onChange.
 */
export function Input() {
  /**
   * Handler déclenché lors du changement de valeur.
   * @param e - Événement de changement sur un input HTML
   */
  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    console.log(e.target.value);
  };

  return <input onChange={handleChange} />;
}
```

---

## 5) Étendre les props HTML

```tsx
/**
 * Props pour le composant Link, hérite des props d'un <a>
 * @property highlight - Met le lien en surbrillance
 */
type LinkProps = React.ComponentPropsWithoutRef<"a"> & {
  highlight?: boolean;
};

/**
 * Lien personnalisé avec option de surbrillance.
 * @param props - Toutes les props valides d’un <a> + highlight
 */
export function Link({ highlight, ...props }: LinkProps) {
  return <a {...props} className={highlight ? "text-blue-600" : ""} />;
}
```

---

## 6) Différence entre `type` et `interface`

- **`type`** : sert à créer un alias (peut être primitif, union, intersection, tuple, etc.).

  ```ts

  ```

type Status = "success" | "error";

````
- **`interface`** : sert surtout pour définir la forme d’un objet, peut être étendue ou fusionnée.
```ts
interface User { id: number; name: string; }
interface User { email?: string; } // fusion automatique
````

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

- Définir un `type` ou `interface` pour les **props** et ajouter des JSDoc.
- Utiliser `?` pour rendre une prop **optionnelle**.
- `children` = contenu entre les balises.
- Bien typer les **événements** (`ChangeEvent`, `MouseEvent`).
- `ComponentPropsWithoutRef<'tag'>` pour réutiliser les props HTML.
- Documenter aussi les **fonctions** avec `@param`, `@returns`, `@example`.
