# Les outils (tools) de Claude Code

## Qu'est-ce qu'un outil ?

Dans Claude Code, un **outil** (*tool*) est une capacité concrète que Claude peut utiliser pour interagir avec ton environnement — lire un fichier, exécuter une commande, faire une recherche web, etc.

Sans outils, Claude ne peut que répondre en texte. Avec les outils, il devient un **agent** : il prend des actions réelles sur ton projet.

```
Claude (LLM) + Outils = Agent capable d'agir
     ↓               ↓
  Réponses         Actions
  en texte         sur les fichiers,
                   le terminal, le web...
```

> **Concept clé :** Claude décide *lui-même* quel outil utiliser en fonction de ta demande. Tu n'as généralement pas à les appeler manuellement — tu parles en langage naturel et Claude choisit l'outil adapté.

---

## Les outils intégrés

Claude Code est livré avec un ensemble d'outils natifs regroupés par catégorie.

### Fichiers et code

| Outil | Description | Permission requise |
|-------|-------------|-------------------|
| `Read` | Lit le contenu d'un fichier avec numéros de ligne. Gère aussi images, PDF, notebooks Jupyter | Non |
| `Write` | Crée ou écrase un fichier entier | **Oui** |
| `Edit` | Modifie une portion précise d'un fichier existant (remplacement exact de chaîne) | **Oui** |
| `Glob` | Cherche des fichiers par pattern (ex: `**/*.ts`) | Non |
| `Grep` | Cherche un pattern dans le contenu des fichiers (basé sur `ripgrep`) | Non |
| `NotebookEdit` | Modifie les cellules d'un notebook Jupyter | **Oui** |

#### Exemple — Read + Edit en pratique

Claude reçoit : *"Ajoute une vérification du paramètre `email` dans la fonction `createUser`"*

```
1. Claude utilise Glob  → trouve les fichiers contenant "createUser"
2. Claude utilise Grep  → localise la fonction précise
3. Claude utilise Read  → lit le fichier pour voir le contexte
4. Claude utilise Edit  → modifie uniquement la fonction ciblée
```

---

### Terminal et système

| Outil | Description | Permission requise |
|-------|-------------|-------------------|
| `Bash` | Exécute des commandes shell. Timeout 2 min par défaut (max 10 min) | **Oui** |
| `PowerShell` | Exécute des commandes PowerShell (Windows principalement) | **Oui** |
| `Monitor` | Lance une commande en arrière-plan et remonte chaque ligne de sortie à Claude en temps réel | **Oui** |

> **Note sur Bash :** les variables d'environnement ne persistent pas entre deux appels. Si tu fais `export VAR=x` dans une commande, elle ne sera pas disponible dans la suivante.

#### Exemple — Bash

```bash
# Claude peut exécuter ceci pour lancer les tests et lire les erreurs :
npm run test -- --watchAll=false
```

---

### Recherche web

| Outil | Description | Permission requise |
|-------|-------------|-------------------|
| `WebSearch` | Lance une recherche web via le moteur Anthropic | **Oui** |
| `WebFetch` | Récupère le contenu d'une URL et en extrait les informations pertinentes | **Oui** |

> **Comment ils fonctionnent ensemble :** `WebSearch` retourne des titres et URLs. `WebFetch` est utilisé ensuite pour lire le contenu d'une page trouvée.

---

### Agents et tâches

| Outil | Description | Permission requise |
|-------|-------------|-------------------|
| `Agent` | Crée un **sous-agent** dans une fenêtre de contexte séparée pour déléguer une tâche | Non |
| `TaskCreate` | Crée une nouvelle tâche dans la liste des tâches de la session | Non |
| `TaskList` | Liste toutes les tâches avec leur statut | Non |
| `TaskUpdate` | Met à jour le statut d'une tâche | Non |
| `TaskGet` | Récupère les détails d'une tâche spécifique | Non |
| `TaskStop` | Arrête une tâche en arrière-plan | Non |

---

### Planification et navigation

| Outil | Description | Permission requise |
|-------|-------------|-------------------|
| `EnterPlanMode` | Passe en mode plan — Claude réfléchit sans modifier de fichiers | Non |
| `ExitPlanMode` | Présente le plan et demande validation avant d'agir | **Oui** |
| `EnterWorktree` | Crée un worktree Git isolé pour travailler sans affecter la branche principale | Non |
| `ExitWorktree` | Quitte le worktree et revient au dossier d'origine | Non |
| `AskUserQuestion` | Pose une question à choix multiples pour clarifier une ambiguïté | Non |

---

### Divers

| Outil | Description | Permission requise |
|-------|-------------|-------------------|
| `LSP` | Intelligence de code via serveur de langage : navigation, définitions, références, erreurs de types | Non |
| `Skill` | Exécute un *skill* (flux de travail personnalisé) dans la conversation | **Oui** |
| `WebSearch` | Recherche sur le web | **Oui** |
| `PushNotification` | Envoie une notification push (desktop ou mobile) quand une tâche longue se termine | Non |
| `CronCreate` | Planifie une tâche répétitive ou ponctuelle dans la session | Non |
| `ScheduleWakeup` | Reprogramme la prochaine itération d'un `/loop` auto-cadencé | Non |

---

## Illustration : cycle d'une tâche agentique

```
┌─────────────────────────────────────────────────────┐
│                  Ta demande (prompt)                 │
│   "Corrige le bug dans le module d'authentification" │
└─────────────────────┬───────────────────────────────┘
                      │
                      ▼
        ┌─────────────────────────┐
        │   Claude analyse        │
        │   et choisit les outils │
        └────────────┬────────────┘
                     │
        ┌────────────▼────────────┐
        │  Glob + Grep            │  ← Trouve les fichiers concernés
        └────────────┬────────────┘
                     │
        ┌────────────▼────────────┐
        │  Read                   │  ← Lit le code pour comprendre
        └────────────┬────────────┘
                     │
        ┌────────────▼────────────┐
        │  Bash                   │  ← Exécute les tests pour reproduire
        └────────────┬────────────┘
                     │
        ┌────────────▼────────────┐
        │  Edit                   │  ← Applique la correction
        └────────────┬────────────┘
                     │
        ┌────────────▼────────────┐
        │  Bash                   │  ← Relance les tests pour valider
        └─────────────────────────┘
```

---

## Permissions des outils

Certains outils nécessitent une **confirmation** avant d'agir (ceux marqués "Oui" dans les tableaux). Claude Code affiche une demande de permission pour :

- Modifier ou créer des fichiers (`Write`, `Edit`)
- Exécuter des commandes shell (`Bash`)
- Accéder au web (`WebSearch`, `WebFetch`)

### Modes de permission disponibles

| Mode | Comportement |
|------|-------------|
| `default` | Demande confirmation pour les actions sensibles |
| `acceptEdits` | Accepte automatiquement les modifications de fichiers |
| `plan` | Claude planifie sans jamais modifier quoi que ce soit |
| `auto` | Accepte automatiquement les actions jugées sûres |
| `bypassPermissions` | Saute toutes les permissions (⚠️ dangereux) |

Changer de mode en session : `Shift+Tab` pour cycler entre les modes.

---

## Les commandes CLI de base

Ces commandes se tapent **dans le terminal**, avant de lancer une session.

### Commandes principales

| Commande | Description | Exemple |
|----------|-------------|---------|
| `claude` | Démarre une session interactive | `claude` |
| `claude "question"` | Démarre avec une question initiale | `claude "explique ce projet"` |
| `claude -p "question"` | Mode non-interactif : répond et quitte | `claude -p "résume ce fichier"` |
| `claude -c` | Reprend la dernière conversation du dossier | `claude -c` |
| `claude -r "nom"` | Reprend une session par nom ou ID | `claude -r "auth-refactor"` |
| `claude update` | Met à jour Claude Code vers la dernière version | `claude update` |
| `claude auth login` | Se connecte à son compte Anthropic | `claude auth login` |
| `claude auth logout` | Se déconnecte | `claude auth logout` |
| `claude auth status` | Affiche le statut d'authentification | `claude auth status` |

### Flags utiles

| Flag | Description | Exemple |
|------|-------------|---------|
| `--model` | Choisit le modèle pour la session | `claude --model claude-opus-4-7` |
| `--permission-mode` | Démarre dans un mode de permission précis | `claude --permission-mode plan` |
| `--verbose` | Affiche les détails de chaque tour de conversation | `claude --verbose` |
| `--print`, `-p` | Mode non-interactif (script/CI) | `claude -p "query"` |
| `--continue`, `-c` | Reprend la dernière conversation | `claude -c` |
| `--resume`, `-r` | Reprend une session par nom ou ID | `claude -r "ma-session"` |
| `--add-dir` | Ajoute un dossier supplémentaire accessible | `claude --add-dir ../lib` |
| `--tools` | Restreint les outils disponibles | `claude --tools "Bash,Read,Edit"` |
| `--max-turns` | Limite le nombre de tours (mode script) | `claude -p --max-turns 5 "query"` |

#### Exemple — Utilisation en pipeline (mode script)

```bash
# Analyser les dernières lignes de logs
tail -200 app.log | claude -p "signale les anomalies critiques"

# Vérifier les fichiers modifiés pour des failles de sécurité
git diff main --name-only | claude -p "analyse ces fichiers pour des problèmes de sécurité"

# Limiter le budget API en CI
claude -p --max-budget-usd 2.00 "lance les tests et corrige les erreurs"
```

---

## Les commandes slash (in-session)

Ces commandes se tapent **pendant une session interactive**, précédées d'un `/`.

### Les incontournables

| Commande | Description |
|----------|-------------|
| `/help` | Affiche l'aide et la liste des commandes disponibles |
| `/clear` | Démarre une nouvelle conversation (garde la mémoire projet) |
| `/compact` | Résume la conversation pour libérer du contexte |
| `/model [modèle]` | Change le modèle utilisé en cours de session |
| `/plan [description]` | Entre en mode plan avant une grande modification |
| `/diff` | Affiche les changements non committés avec un viewer interactif |
| `/permissions` | Gère les règles allow/deny pour les outils |
| `/memory` | Édite les fichiers `CLAUDE.md` de mémoire du projet |
| `/init` | Génère un fichier `CLAUDE.md` starter pour le projet |
| `/resume [session]` | Reprend une conversation précédente |
| `/exit` ou `/quit` | Quitte la session |

### Commandes de review et qualité

| Commande | Description |
|----------|-------------|
| `/review [PR]` | Revue d'une pull request localement |
| `/security-review` | Analyse les changements en attente pour des failles de sécurité |
| `/code-review` | Revue du diff actuel pour des bugs de logique |

### Commandes pratiques

| Commande | Description |
|----------|-------------|
| `/rewind` | Annule les changements et revient à un point précédent |
| `/compact` | Compresse le contexte de la conversation |
| `/context` | Visualise l'utilisation actuelle de la fenêtre de contexte |
| `/tasks` | Liste et gère les tâches en arrière-plan |
| `/mcp` | Gère les serveurs MCP connectés |
| `/config` | Ouvre l'interface de paramètres (thème, modèle, etc.) |
| `/doctor` | Diagnostique l'installation et la configuration |
| `/btw <question>` | Pose une question rapide sans polluer l'historique |
| `/copy [N]` | Copie la dernière réponse dans le presse-papier |

### Commandes de session

| Commande | Description |
|----------|-------------|
| `/background` | Détache la session pour la faire tourner en arrière-plan |
| `/rename [nom]` | Renomme la session courante |
| `/branch [nom]` | Crée une branche de la conversation à ce point |
| `/loop [intervalle] [prompt]` | Répète un prompt en boucle à intervalles réguliers |
| `/schedule [description]` | Crée des routines planifiées sur infrastructure Anthropic |

---

## Les raccourcis clavier essentiels

### Contrôle général

| Raccourci | Action |
|-----------|--------|
| `Ctrl+C` | Interrompt une opération en cours (ou vide l'input) |
| `Ctrl+D` | Quitte Claude Code |
| `Esc` | Interrompt la réponse de Claude |
| `Esc` + `Esc` | Vide le draft ou ouvre le menu de rembobinage |
| `Ctrl+R` | Recherche dans l'historique des commandes |
| `Ctrl+L` | Redessine l'écran (utile si l'affichage est cassé) |
| `Ctrl+O` | Bascule le viewer de transcript (voir les appels d'outils) |
| `Ctrl+T` | Bascule la liste des tâches |
| `Shift+Tab` | Cycle entre les modes de permission |

### Saisie multiligne

| Méthode | Raccourci |
|---------|-----------|
| Universel | `\` + `Entrée` |
| iTerm2 / Terminal macOS / VS Code | `Shift+Entrée` |
| Universel (terminal) | `Ctrl+J` |

### Modèle et réglages

| Raccourci | Action |
|-----------|--------|
| `Option+P` (macOS) / `Alt+P` (Win/Linux) | Change de modèle sans effacer le prompt |
| `Option+T` (macOS) / `Alt+T` (Win/Linux) | Active/désactive le mode "extended thinking" |
| `Option+O` (macOS) / `Alt+O` (Win/Linux) | Active/désactive le mode rapide (*fast mode*) |

### Préfixes spéciaux dans le prompt

| Préfixe | Comportement |
|---------|-------------|
| `/` | Ouvre la liste des commandes slash |
| `!` | Mode shell — exécute directement sans passer par Claude |
| `@` | Autocomplétion de chemin de fichier |

#### Exemple — Mode shell avec `!`

```bash
# Dans une session Claude Code, tu peux taper directement :
! git status
! npm test
! ls -la src/

# La sortie est ajoutée au contexte de la conversation
```

---

## Ajouter des outils avec MCP

**MCP** (*Model Context Protocol*) est un standard ouvert qui permet de connecter des outils externes à Claude Code. Exemples d'outils MCP disponibles :

- Google Drive, Gmail, Google Calendar
- Jira, Linear, GitHub Issues
- Slack, Discord
- Des outils maison personnalisés

```bash
# Gérer les serveurs MCP depuis le terminal
claude mcp

# Voir les serveurs connectés dans une session
/mcp
```

---

## Résumé visuel

```
CLAUDE CODE
    │
    ├── Outils fichiers ──► Read, Write, Edit, Glob, Grep, NotebookEdit
    │
    ├── Outils terminal ──► Bash, PowerShell, Monitor
    │
    ├── Outils web ───────► WebSearch, WebFetch
    │
    ├── Outils agents ────► Agent, TaskCreate/List/Update/Stop
    │
    ├── Outils planif. ───► EnterPlanMode, ExitPlanMode, EnterWorktree
    │
    └── Outils divers ────► LSP, Skill, PushNotification, CronCreate
```

---

## Points clés à retenir

- Un **outil** = une action concrète que Claude peut effectuer (lire, écrire, exécuter, chercher).
- Claude choisit **automatiquement** les outils — tu n'as pas à les nommer dans ta demande.
- Les outils sensibles (écriture, shell, web) nécessitent une **permission** que tu peux accorder ou refuser.
- Les **commandes CLI** (`claude`, `claude -p`, `claude -c`) se tapent dans le terminal.
- Les **commandes slash** (`/clear`, `/plan`, `/model`) se tapent pendant une session.
- Le **mode shell** (`!commande`) permet d'exécuter du shell directement depuis Claude Code.
- Les **outils MCP** permettent d'étendre Claude avec tes propres intégrations.

---

## 📚 Sources

- [Référence complète des outils — Claude Code](https://code.claude.com/docs/en/tools-reference)
- [Référence CLI — Claude Code](https://code.claude.com/docs/en/cli-reference)
- [Mode interactif et raccourcis clavier](https://code.claude.com/docs/en/interactive-mode)
- [Référence des commandes slash](https://code.claude.com/docs/en/commands)
- [MCP — Model Context Protocol](https://code.claude.com/docs/fr/mcp)
