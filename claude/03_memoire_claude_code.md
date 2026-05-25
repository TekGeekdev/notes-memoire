# La mémoire dans Claude Code

## Le problème de base : chaque session repart de zéro

Par défaut, Claude Code n'a **aucune mémoire entre les sessions**. Chaque fois que tu ouvres une nouvelle session, Claude ne se souvient pas :

- Des conventions de code de ton projet
- Des commandes de build ou de test
- De tes préférences personnelles
- Des décisions d'architecture prises la semaine dernière

Sans mémoire, tu dois **ré-expliquer le contexte à chaque session** — ce qui est lent et frustrant.

---

## Les deux systèmes de mémoire

Claude Code propose deux mécanismes complémentaires pour garder le contexte entre les sessions :

```
┌─────────────────────────────────────────────────────────┐
│                   MÉMOIRE CLAUDE CODE                   │
├──────────────────────────┬──────────────────────────────┤
│      CLAUDE.md           │       Auto Memory            │
│  (tu l'écris toi-même)   │  (Claude l'écrit lui-même)   │
├──────────────────────────┼──────────────────────────────┤
│ Instructions, règles,    │ Apprentissages, préférences  │
│ conventions d'équipe     │ découvertes en session        │
│                          │                              │
│ Portée : projet, user,   │ Portée : par dépôt Git       │
│ organisation             │ (partagé entre worktrees)    │
│                          │                              │
│ Chargé : à chaque        │ Chargé : 200 premières       │
│ session en entier        │ lignes à chaque session       │
└──────────────────────────┴──────────────────────────────┘
```

|             | CLAUDE.md                            | Auto Memory                           |
| ----------- | ------------------------------------ | ------------------------------------- |
| **Auteur**  | Toi                                  | Claude                                |
| **Contenu** | Instructions et règles               | Apprentissages et patterns            |
| **Portée**  | Projet, utilisateur, ou organisation | Par dépôt Git                         |
| **Chargé**  | Entièrement à chaque session         | 200 premières lignes / 25 Ko          |
| **Utilité** | Conventions, architecture, workflows | Commandes de build, insights de debug |

---

## 1. CLAUDE.md — La mémoire que tu écris

### C'est quoi ?

Un fichier Markdown que Claude lit **au début de chaque session**. C'est l'endroit où tu écris tout ce que tu devrais autrement ré-expliquer à Claude à chaque fois.

### Quand l'écrire ?

- Claude fait la **même erreur deux fois**
- Tu tapes **la même correction** qu'à la session précédente
- Un nouveau coéquipier aurait besoin du même contexte pour être productif
- Une code review révèle quelque chose que Claude aurait dû savoir

### Exemple de CLAUDE.md pour un projet React/TypeScript

```markdown
# Mon Projet

## Stack technique

- React 18 + TypeScript + Vite
- Tests : Vitest + Testing Library
- Style : Tailwind CSS (pas de CSS modules)

## Commandes essentielles

- `npm run dev` — serveur de développement
- `npm test` — lance les tests en mode watch
- `npm run build` — build de production

## Conventions de code

- Utiliser `const` par défaut, jamais `var`
- Les composants React : PascalCase, un fichier par composant
- Les hooks personnalisés commencent par `use`
- Toujours typer les props des composants avec une `interface`, pas un `type`

## Architecture

- `src/components/` — composants réutilisables
- `src/pages/` — pages (un dossier par route)
- `src/hooks/` — hooks personnalisés
- `src/api/` — appels API (utiliser React Query)

## Règles importantes

- Ne jamais pusher directement sur `main`
- Toujours lancer `npm test` avant de committer
- Les clés API ne vont jamais dans le code — utiliser `.env.local`
```

---

## Les emplacements de CLAUDE.md

Les fichiers CLAUDE.md peuvent être placés à plusieurs niveaux, chacun avec une portée différente. Ils sont chargés du plus général au plus spécifique :

```
┌─────────────────────────────────────────────────────────────┐
│  Niveau 1 — Organisation (IT/DevOps)                        │
│  macOS:   /Library/Application Support/ClaudeCode/CLAUDE.md │
│  Linux:   /etc/claude-code/CLAUDE.md                        │
│  Windows: C:\Program Files\ClaudeCode\CLAUDE.md             │
│  → S'applique à TOUS les utilisateurs de la machine         │
├─────────────────────────────────────────────────────────────┤
│  Niveau 2 — Utilisateur (tes préférences perso)             │
│  ~/.claude/CLAUDE.md                                        │
│  → S'applique à TOUS tes projets                            │
├─────────────────────────────────────────────────────────────┤
│  Niveau 3 — Projet (partagé via Git avec l'équipe)          │
│  ./CLAUDE.md  ou  ./.claude/CLAUDE.md                       │
│  → S'applique uniquement à CE projet                        │
├─────────────────────────────────────────────────────────────┤
│  Niveau 4 — Local (tes préférences perso sur ce projet)     │
│  ./CLAUDE.local.md  (à mettre dans .gitignore !)            │
│  → Uniquement pour toi, pas committé                        │
└─────────────────────────────────────────────────────────────┘
```

### Exemple — CLAUDE.md utilisateur (`~/.claude/CLAUDE.md`)

```markdown
# Mes préférences personnelles

- Toujours répondre en français
- Préférer `pnpm` à `npm`
- Pour les commits Git : utiliser le format Conventional Commits
- J'utilise Fish shell (pas bash)
```

### Exemple — CLAUDE.local.md (personnel, non versionné)

```markdown
# Config locale perso (ne pas committer)

- URL de dev local : http://localhost:3000
- Base de données de test : postgresql://localhost/myapp_dev
- Token d'API de test : sk-dev-xxxx (jamais en prod)
```

Pour les préférences personnelles privées par projet qui ne doivent pas être validées dans le contrôle de version, créez un CLAUDE.local.md à la racine du projet. Il se charge aux côtés de CLAUDE.md et est traité de la même manière. Ajoutez CLAUDE.local.md à votre .gitignore pour qu’il ne soit pas validé => exécuter /init et choisir l’option personnelle le fait pour vous.

---

## Organiser les règles avec `.claude/rules/`

Pour les projets complexes, tu peux éclater les instructions en plusieurs fichiers dans `.claude/rules/` :

```
mon-projet/
├── CLAUDE.md                    ← instructions générales
└── .claude/
    └── rules/
        ├── code-style.md        ← style de code
        ├── testing.md           ← conventions de test
        └── security.md          ← règles de sécurité
```

### Règles ciblées par chemin de fichier (frontmatter `paths`)

Une règle peut ne s'appliquer que quand Claude travaille sur certains fichiers :

```markdown
---
paths:
  - 'src/api/**/*.ts'
---

# Règles pour les endpoints API

- Toujours valider les entrées avec Zod
- Utiliser le format d'erreur standard `{ error: string, code: number }`
- Documenter chaque endpoint avec un commentaire JSDoc
```

Ces règles conditionnelles ne s'appliquent que lorsque Claude travaille avec des fichiers correspondant aux patterns spécifiés. Les règles sans champ paths sont chargées inconditionnellement et s'appliquent à tous les fichiers. Les règles scopées par chemin se déclenchent quand Claude lit des fichiers correspondant au pattern, pas à chaque utilisation d'outil.

| Pattern         | Fichiers ciblés               |
| --------------- | ----------------------------- |
| `**/*.ts`       | Tous les fichiers TypeScript  |
| `src/**/*`      | Tout ce qui est sous `src/`   |
| `*.md`          | Fichiers Markdown à la racine |
| `**/*.{ts,tsx}` | TypeScript et TSX partout     |

---

## Créer son CLAUDE.md automatiquement

Au lieu de l'écrire manuellement, Claude peut analyser ton projet et générer un CLAUDE.md :

```bash
# Dans une session Claude Code, tapez :
/init
```

Claude va :

1. Analyser la structure de ton projet
2. Détecter les commandes de build et de test
3. Identifier les conventions déjà en place
4. Générer un `CLAUDE.md` de départ

> Si un `CLAUDE.md` existe déjà, `/init` propose des améliorations sans écraser l'existant.

---

## Importer d'autres fichiers dans CLAUDE.md

Tu peux référencer d'autres fichiers depuis ton CLAUDE.md avec la syntaxe `@chemin/vers/fichier` :

```markdown
# CLAUDE.md

Voir @README.md pour la présentation du projet.
Commandes npm disponibles : @package.json

## Instructions supplémentaires

- Workflow Git : @docs/git-workflow.md
```

> Les fichiers importés sont chargés dans le contexte au démarrage de la session — ils consomment des tokens comme le reste.

---

## 2. Auto Memory — La mémoire que Claude écrit

### C'est quoi ?

Un système où Claude prend **automatiquement des notes** sur ce qu'il apprend au fil des sessions : commandes de build, préférences détectées, insights de débogage, décisions d'architecture...

Tu n'as rien à faire — Claude décide lui-même ce qui mérite d'être mémorisé.

### Où sont stockés les fichiers ?

```
~/.claude/projects/<nom-du-projet>/memory/
├── MEMORY.md          ← index concis, chargé à chaque session
├── debugging.md       ← notes de débogage accumulées
├── api-conventions.md ← patterns d'API détectés
└── ...                ← autres fichiers créés par Claude
```

> Le nom du dossier `<nom-du-projet>` est dérivé du dépôt Git. Tous les worktrees d'un même dépôt **partagent** la même mémoire auto.

### Comment Claude l'utilise ?

```
Début de session
      │
      ▼
Claude charge les 200 premières lignes de MEMORY.md
(ou 25 Ko, selon ce qui arrive en premier)
      │
      ▼
Pendant la session :
  - Tu corriges Claude → il note la correction
  - Tu mentionnes une commande → il la note
  - Tu exprimes une préférence → il la retient
      │
      ▼
Claude écrit / met à jour les fichiers memory/ en temps réel
```

Quand tu vois **"Writing memory"** ou **"Recalled memory"** dans l'interface, c'est Claude qui lit ou écrit dans `~/.claude/projects/<projet>/memory/`.

### Activer / désactiver l'auto memory

L'auto memory est **activée par défaut**. Pour la désactiver :

**Depuis une session :**

```
/memory   ← ouvre le menu → toggle auto memory
```

**Dans les paramètres du projet** (`.claude/settings.json`) :

```json
{
  "autoMemoryEnabled": false
}
```

**Via variable d'environnement :**

```bash
export CLAUDE_CODE_DISABLE_AUTO_MEMORY=1
```

### Changer l'emplacement de stockage

Par défaut dans `~/.claude/projects/`. Pour utiliser un autre dossier, ajoute dans `~/.claude/settings.json` :

```json
{
  "autoMemoryDirectory": "~/mes-notes-claude"
}
```

> Le chemin doit être absolu ou commencer par `~/`.

---

## Gérer sa mémoire avec `/memory`

La commande `/memory` dans une session permet de :

- Voir tous les fichiers CLAUDE.md chargés pour la session courante
- Activer ou désactiver l'auto memory
- Ouvrir et éditer les fichiers memory dans ton éditeur
- Accéder au dossier de mémoire auto

```bash
# Dans une session Claude Code :
/memory
```

Tu peux aussi demander à Claude directement :

```
"Souviens-toi que j'utilise toujours pnpm et jamais npm"
→ Claude sauvegarde dans l'auto memory

"Ajoute ça dans CLAUDE.md"
→ Claude écrit dans le fichier CLAUDE.md du projet
```

---

## Bonnes pratiques pour CLAUDE.md

### Garder le fichier court et précis

- **Cible : moins de 200 lignes** — au-delà, Claude suit moins bien les instructions
- Préfère plusieurs fichiers ciblés (`rules/`) plutôt qu'un seul fichier géant

### Écrire des instructions vérifiables

| Trop vague                   | Concret et utile                                         |
| ---------------------------- | -------------------------------------------------------- |
| "Formate le code proprement" | "Utilise 2 espaces d'indentation, jamais de tabulations" |
| "Teste tes changements"      | "Lance `npm test` avant chaque commit"                   |
| "Organise bien les fichiers" | "Les handlers API vont dans `src/api/handlers/`"         |

### Éviter les contradictions

Si deux fichiers CLAUDE.md donnent des instructions contradictoires, Claude choisit arbitrairement. Vérifie régulièrement la cohérence entre :

- `CLAUDE.md` racine
- `CLAUDE.local.md`
- Les fichiers dans `.claude/rules/`

---

## Illustration : flux complet de mémoire

```
Première session
     │
     ├─ Tu crées CLAUDE.md (ou /init le génère)
     ├─ Claude lit les instructions
     └─ Claude mémorise les découvertes → auto memory

Sessions suivantes
     │
     ├─ Claude charge CLAUDE.md (instructions durables)
     ├─ Claude charge MEMORY.md (apprentissages passés)
     └─ Claude reprend là où il en était, sans ré-explication
```

---

## Résolution des problèmes courants

### "Claude ne suit pas mes instructions CLAUDE.md"

1. **Vérifie que le fichier est chargé** : lance `/memory` et regarde si ton fichier apparaît dans la liste
2. **Vérifie l'emplacement** : le fichier doit être dans un dossier parent du répertoire de travail
3. **Rends les instructions plus précises** : "2 espaces" plutôt que "bien indenté"
4. **Cherche des contradictions** : deux règles opposées se neutralisent

### "Les instructions disparaissent après `/compact`"

- Le `CLAUDE.md` à la racine du projet **survit** à la compaction — il est rechargé automatiquement
- Les `CLAUDE.md` dans des sous-dossiers ne sont pas rechargés automatiquement, seulement quand Claude lit un fichier dans ce sous-dossier

### "Je ne sais pas ce que l'auto memory a sauvegardé"

Lance `/memory` et ouvre le dossier de mémoire auto — tout est en Markdown lisible et modifiable.

---

## Points clés à retenir

- Chaque session Claude Code commence avec une **fenêtre de contexte vide** — la mémoire est ce qui permet la continuité.
- **CLAUDE.md** = ce que _tu_ écris pour donner des instructions durables à Claude.
- **Auto Memory** = ce que _Claude_ écrit pour mémoriser ses apprentissages.
- Les deux sont **chargés automatiquement** au début de chaque session.
- `/init` génère un CLAUDE.md de départ en analysant ton projet.
- `/memory` est la commande centrale pour gérer les deux systèmes.
- Les fichiers `CLAUDE.local.md` sont pour tes préférences perso — **à mettre dans `.gitignore`**.

---

## 📚 Sources

- [Mémoire Claude Code — Documentation officielle](https://code.claude.com/docs/en/memory)
- [Commandes slash Claude Code](https://code.claude.com/docs/en/commands)
- [Paramètres Claude Code](https://code.claude.com/docs/en/settings)
- [Skills et workflows personnalisés](https://code.claude.com/docs/en/skills)
