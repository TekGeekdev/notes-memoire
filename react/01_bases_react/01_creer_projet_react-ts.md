# 1) Pré-requis

- **Node.js ≥ 18** (LTS recommandé).  
  Vérifie : `node -v`

- Un gestionnaire de paquets : **npm** (par défaut), ou **pnpm** / **yarn**.

# 2) Créer le projet

## Avec npm

```bash
# ① Génère le squelette
npm create vite@latest
# variantes :
# --template react-swc-ts  (même résultat TypeScript, mais transpile via SWC, plus rapide)

# ② Installe les dépendances
cd my-react-ts
npm install

# ③ Lance en dev
npm run dev
```

## Avec pnpm (optionnel, plus rapide)

```bash
pnpm create vite my-react-ts --template react-ts
cd my-react-ts
pnpm install
pnpm dev
```

## Avec yarn (si tu préfères)

```bash
yarn create vite my-react-ts --template react-ts
cd my-react-ts
yarn
yarn dev
```

### 💡 **Choix du template**

- `react-ts` : Vite + React + TypeScript + transpile via **esbuild** (par Vite)

- `react-swc-ts` : identique, mais Vite utilise **SWC** côté dev/build → **compilations plus rapides**.  
  Dans les deux cas, **TypeScript** est vérifié par `tsc` (types), et le rendu est le même.

### 3) Arborescence minimale attendue

```css
my-react-ts/
  ├─ index.html
  ├─ package.json
  ├─ tsconfig.json
  ├─ tsconfig.node.json
  ├─ vite.config.ts
  └─ src/
      ├─ main.tsx
      ├─ App.tsx
      └─ assets/...
```

## 4) Fichiers clés (ce que tu verras)

## `src/main.tsx`

Point d’entrée qui monte l’app :

```ts
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App.tsx";

ReactDOM.createRoot(document.getElementById("root")!).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

## `src/App.tsx`

Composant racine TypeScript/TSX :

```tsx
type HelloProps = { name?: string };

export default function App({ name = "Tekgeek_dev" }: HelloProps) {
  return (
    <div style={{ padding: 24 }}>
      <h1>React + TypeScript + Vite</h1>
      <p>Bienvenue, {name} 👋</p>
    </div>
  );
}
```

## 5) Scripts utiles (dans `package.json`)

```json
{
  "scripts": {
    "dev": "vite",
    "build": "tsc -b && vite build",
    "preview": "vite preview",
    "typecheck": "tsc --noEmit"
  }
}
```

- `dev` : serveur de dev avec HMR

- `build` : build de production (avec vérification TS)

- `preview` : sert le build pour tester

- `typecheck` : vérifie les types sans lancer Vite

## 6) (Optionnel) ESLint + Prettier rapidement

```bash
# ESLint officiel React + TS
npm i -D eslint @eslint/js typescript-eslint eslint-plugin-react eslint-plugin-react-hooks
# Prettier
npm i -D prettier eslint-config-prettier
```
