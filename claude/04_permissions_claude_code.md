# Les permissions dans Claude Code

## Pourquoi les permissions ?

Claude Code est un agent qui peut **lire, modifier des fichiers, exécuter des commandes shell et accéder au web**. Sans système de permission, il pourrait faire des choses indésirables sans te demander.

Les permissions permettent de définir précisément **ce que Claude peut faire automatiquement** et **ce qui doit être validé** avant d'être exécuté.

> **Point clé :** les permissions sont appliquées par Claude Code (le logiciel), pas par le modèle IA. Les instructions dans ton prompt ou ton `CLAUDE.md` influencent ce que Claude *essaie* de faire, mais seules les règles de permission contrôlent ce qu'il *peut* faire.

---

## Le système de permission à trois niveaux

Chaque outil appartient à une catégorie avec un comportement par défaut différent :

| Type d'outil | Exemple | Confirmation requise | Comportement "Ne plus demander" |
|---|---|---|---|
| **Lecture seule** | Lire un fichier, `Grep` | Non | N/A |
| **Commandes Bash** | Exécution shell | **Oui** | Permanent par projet et commande |
| **Modification de fichier** | `Edit`, `Write` | **Oui** | Jusqu'à la fin de la session |

---

## Les trois types de règles : Allow / Ask / Deny

```
┌──────────────────────────────────────────────────────────┐
│              Ordre d'évaluation des règles               │
│                                                          │
│   DENY  ──►  ASK  ──►  ALLOW                             │
│                                                          │
│  La première règle qui correspond gagne.                 │
│  Les règles Deny ont toujours la priorité.               │
└──────────────────────────────────────────────────────────┘
```

| Règle | Effet |
|-------|-------|
| **Allow** | Claude utilise l'outil sans demander de confirmation |
| **Ask** | Claude demande confirmation à chaque utilisation |
| **Deny (nom seul)** | Supprime l'outil du contexte — Claude ne le voit plus du tout |
| **Deny (avec pattern)** | Laisse l'outil disponible, bloque uniquement les appels correspondants |

Exemples :
- `Deny: Bash` → Claude ne peut plus du tout utiliser le shell
- `Deny: Bash(rm *)` → Claude peut utiliser Bash, mais pas les commandes `rm`

Gérer les règles depuis une session :
```
/permissions
```

---

## Les modes de permission

Au lieu de configurer règle par règle, tu peux basculer dans un **mode global** qui change le comportement général de Claude Code.

| Mode | Comportement |
|------|-------------|
| `default` | Demande confirmation au premier usage de chaque outil |
| `acceptEdits` | Accepte automatiquement les modifications de fichiers |
| `plan` | Claude explore sans jamais modifier quoi que ce soit |
| `auto` | Approuve automatiquement les actions jugées sûres (recherche preview) |
| `dontAsk` | Refuse automatiquement sauf si pré-approuvé dans `/permissions` |
| `bypassPermissions` | Saute toutes les confirmations ⚠️ |

Changer de mode en session :
```
Shift+Tab  →  cycle entre les modes
```

Ou au lancement :
```bash
claude --permission-mode plan
claude --permission-mode acceptEdits
```

> **Attention :** `bypassPermissions` supprime toutes les confirmations, y compris les écritures dans `.git`, `.claude`, `.vscode`. À n'utiliser que dans des environnements isolés (containers, VMs).

---

## La syntaxe des règles de permission

Une règle suit le format : `Outil` ou `Outil(specifier)`

### Cibler tous les usages d'un outil

```
Bash          →  toutes les commandes Bash
WebFetch      →  toutes les requêtes web
Read          →  toutes les lectures de fichiers
```

### Cibler un usage précis (avec specifier)

```
Bash(npm run build)          →  exactement "npm run build"
Bash(npm run *)              →  toutes les commandes npm run ...
Read(./.env)                 →  lecture du fichier .env
WebFetch(domain:github.com)  →  requêtes vers github.com uniquement
```

### Wildcards avec `*`

Le `*` peut apparaître n'importe où dans la commande :

```
Bash(npm *)           →  toute commande commençant par "npm "
Bash(* --version)     →  toute commande finissant par "--version"
Bash(git * main)      →  ex: "git checkout main", "git merge main"
```

> **Important :** l'espace avant `*` crée une frontière de mot. `Bash(ls *)` correspond à `ls -la` mais pas à `lsof`. Sans espace (`Bash(ls*)`), les deux correspondent.

---

## Exemples de configuration dans `settings.json`

Les règles se configurent dans `.claude/settings.json` (projet) ou `~/.claude/settings.json` (utilisateur).

### Autoriser certaines commandes sans confirmation

```json
{
  "permissions": {
    "allow": [
      "Bash(npm run *)",
      "Bash(git commit *)",
      "Bash(git status)",
      "Bash(* --version)",
      "Bash(* --help *)"
    ]
  }
}
```

### Bloquer certaines commandes dangereuses

```json
{
  "permissions": {
    "allow": [
      "Bash(git *)"
    ],
    "deny": [
      "Bash(git push *)",
      "Bash(rm *)",
      "Bash(sudo *)"
    ]
  }
}
```

### Restreindre l'accès aux fichiers

```json
{
  "permissions": {
    "deny": [
      "Read(**/.env)",
      "Read(~/.ssh/**)",
      "Edit(/config/prod/**)"
    ]
  }
}
```

---

## Règles par outil — détails

### Bash

Les commandes **lecture seule** sont exécutées sans confirmation dans tous les modes : `ls`, `cat`, `echo`, `pwd`, `head`, `tail`, `grep`, `find`, `wc`, `diff`, `stat`, `git log`, `git status`, etc.

Pour les commandes composées (`&&`, `||`, `;`, `|`), chaque sous-commande est vérifiée indépendamment. Une règle doit correspondre à chaque sous-commande pour que la commande composée soit autorisée.

### Read et Edit

Les règles `Read` et `Edit` suivent la syntaxe **gitignore** pour les chemins :

| Pattern | Signification | Exemple |
|---------|---------------|---------|
| `//chemin` | Chemin absolu depuis la racine | `Read(//etc/hosts)` |
| `~/chemin` | Depuis le dossier home | `Read(~/.zshrc)` |
| `/chemin` | Relatif à la racine du projet | `Edit(/src/**/*.ts)` |
| `chemin` | Relatif au dossier courant | `Read(*.env)` |

> **Attention :** `/Users/alice/fichier` n'est PAS un chemin absolu dans cette syntaxe — c'est relatif à la racine du projet. Pour un chemin absolu, utilise `//Users/alice/fichier`.

Exemples :
```
Read(**/.env)       →  bloque tout fichier .env à n'importe quelle profondeur
Edit(/src/**)       →  autorise l'édition de tout ce qui est dans src/ du projet
Read(~/.ssh/**)     →  bloque la lecture du dossier SSH de l'utilisateur
```

### WebFetch

```
WebFetch(domain:github.com)    →  requêtes vers github.com uniquement
WebFetch(domain:api.stripe.com) →  uniquement l'API Stripe
```

### MCP (outils externes)

```
mcp__puppeteer                  →  tous les outils du serveur "puppeteer"
mcp__puppeteer__puppeteer_navigate  →  uniquement l'outil navigate
```

### Agent (sous-agents)

```
Agent(Explore)     →  le sous-agent Explore
Agent(mon-agent)   →  un sous-agent personnalisé
```

Pour désactiver un agent :
```json
{
  "permissions": {
    "deny": ["Agent(Explore)"]
  }
}
```

---

## Illustration : flux d'évaluation d'une permission

```
Claude veut exécuter : Bash(git push origin main)
              │
              ▼
   ┌─────────────────────┐
   │  Règle Deny ?        │──► OUI → Bloqué (Claude ne peut pas)
   └─────────────────────┘
              │ NON
              ▼
   ┌─────────────────────┐
   │  Règle Ask ?         │──► OUI → Demande confirmation
   └─────────────────────┘
              │ NON
              ▼
   ┌─────────────────────┐
   │  Règle Allow ?       │──► OUI → Exécuté sans confirmation
   └─────────────────────┘
              │ NON
              ▼
      Comportement par défaut
      (demande selon le type d'outil)
```

---

## Priorité des paramètres

Les permissions peuvent être définies à plusieurs niveaux. La priorité va du plus fort au plus faible :

```
1. Managed settings (IT/DevOps — non modifiable)
2. Arguments CLI (--allowedTools, --disallowedTools)
3. .claude/settings.local.json (projet, perso)
4. .claude/settings.json (projet, partagé)
5. ~/.claude/settings.json (utilisateur)
```

> **Règle importante :** si un outil est **refusé à n'importe quel niveau**, aucun autre niveau ne peut l'autoriser. Un `deny` en `settings.json` projet écrase un `allow` dans `~/.claude/settings.json` utilisateur.

---

## Les répertoires de travail

Par défaut, Claude a accès aux fichiers du dossier où il a été lancé. Pour étendre cet accès :

```bash
# Au lancement
claude --add-dir ../lib --add-dir ../shared

# Dans une session
/add-dir ../autreprojet
```

Ou de façon permanente dans `settings.json` :
```json
{
  "permissions": {
    "additionalDirectories": ["../lib", "../shared"]
  }
}
```

> Ajouter un dossier donne accès aux **fichiers** de ce dossier, mais ne charge pas automatiquement la configuration `.claude/` qu'il contient (hooks, settings, etc.).

---

## Gérer les permissions via le CLI

```bash
# Voir les règles actives (interactif)
/permissions

# Lancer en mode plan (aucune modification)
claude --permission-mode plan

# Autoriser des outils spécifiques sans confirmation
claude --allowedTools "Bash(git *)" "Read"

# Interdire des outils
claude --disallowedTools "WebSearch" "Bash(rm *)"

# Restreindre aux seuls outils listés
claude --tools "Bash,Read,Edit"
```

---

## Points clés à retenir

- Les permissions s'évaluent dans l'ordre : **Deny → Ask → Allow** — la première règle qui correspond gagne.
- Un `Deny` sur un nom d'outil seul (`Bash`) **supprime** l'outil du contexte. Un `Deny` avec pattern (`Bash(rm *)`) le laisse disponible et bloque uniquement les appels correspondants.
- Les commandes shell **lecture seule** (`ls`, `cat`, `grep`, etc.) ne demandent jamais de confirmation.
- `Shift+Tab` cycle entre les modes de permission en cours de session.
- Les règles dans les **managed settings** ne peuvent pas être écrasées par les autres niveaux.
- Les chemins dans les règles `Read`/`Edit` suivent la syntaxe gitignore — `/chemin` est relatif au projet, `//chemin` est absolu.

---

## 📚 Sources

- [Permissions — Documentation officielle Claude Code](https://code.claude.com/docs/en/permissions)
- [Modes de permission](https://code.claude.com/docs/en/permission-modes)
- [Sandboxing — isolation OS](https://code.claude.com/docs/en/sandboxing)
- [Hooks — étendre les permissions](https://code.claude.com/docs/en/hooks-guide)
- [Paramètres Claude Code](https://code.claude.com/docs/en/settings)
