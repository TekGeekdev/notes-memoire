# Les Hooks dans Claude Code

## C'est quoi un hook ?

Un **hook** est une commande shell (ou un script) qui s'exécute **automatiquement** à des moments précis du cycle de vie de Claude Code — avant qu'il modifie un fichier, après qu'il répond, quand il a besoin d'input, etc.

```
Sans hook :  Claude agit → résultat (pas de contrôle)
Avec hook :  Claude agit → hook s'exécute → résultat
```

> **Différence clé avec les skills et CLAUDE.md :**
> Les skills et CLAUDE.md *influencent* ce que Claude *décide* de faire.
> Les hooks s'exécutent de façon **déterministe** — ils se déclenchent toujours, peu importe ce que Claude choisit.
>
> Exemple : demander à Claude "formate toujours le code" dans CLAUDE.md, il peut oublier. Un hook `PostToolUse` qui lance Prettier après chaque édition, lui, ne rate jamais.

---

## Les cinq types de hooks

| Type | Description |
|------|-------------|
| `command` | Script shell recevant du JSON sur stdin |
| `http` | Requête POST vers un endpoint HTTP |
| `mcp_tool` | Appel à un outil d'un serveur MCP |
| `prompt` | Évaluation par un modèle Claude (décision oui/non) |
| `agent` | Sous-agent avec accès aux outils pour de la vérification |

Le type le plus courant est `command` — un simple script bash ou shell.

---

## Structure de configuration

Les hooks sont définis dans les fichiers `settings.json` avec trois niveaux d'imbrication :

```json
{
  "hooks": {
    "NOM_EVENEMENT": [
      {
        "matcher": "NomOutil",
        "hooks": [
          {
            "type": "command",
            "command": "/chemin/vers/script.sh"
          }
        ]
      }
    ]
  }
}
```

- **Niveau 1** — l'événement (`PreToolUse`, `PostToolUse`, `Notification`...)
- **Niveau 2** — le matcher (quel outil déclenche le hook)
- **Niveau 3** — le hook lui-même (type + commande)

---

## Où placer ses hooks

| Fichier | Portée | Versionnable |
|---------|--------|-------------|
| `~/.claude/settings.json` | Tous tes projets | Non |
| `.claude/settings.json` | Ce projet uniquement | Oui |
| `.claude/settings.local.json` | Ce projet, perso | Non |
| Frontmatter d'un skill/agent | Pendant l'exécution du skill | Oui |

Voir les hooks actifs depuis une session :
```
/hooks
```

---

## Les événements disponibles

### Événements de session

| Événement | Se déclenche quand |
|-----------|-------------------|
| `SessionStart` | La session commence ou reprend |
| `Setup` | Avec `--init-only` ou `--init`/`--maintenance` en mode `-p` |
| `SessionEnd` | La session se termine |

### Événements par tour

| Événement | Se déclenche quand |
|-----------|-------------------|
| `UserPromptSubmit` | Avant que Claude traite ton message |
| `Stop` | Claude a fini de répondre |
| `StopFailure` | Le tour se termine sur une erreur API |
| `Notification` | Claude attend ton input ou une permission |

### Événements autour des outils

| Événement | Se déclenche quand |
|-----------|-------------------|
| `PreToolUse` | **Avant** l'exécution d'un outil (peut bloquer) |
| `PostToolUse` | **Après** qu'un outil réussit |
| `PostToolUseFailure` | Après qu'un outil échoue |
| `PermissionRequest` | La boîte de dialogue de permission apparaît |
| `PermissionDenied` | Un outil est refusé par le classifieur auto |
| `PostToolBatch` | Après qu'un lot d'outils parallèles se résout |

### Événements fichiers et config

| Événement | Se déclenche quand |
|-----------|-------------------|
| `FileChanged` | Un fichier surveillé change |
| `ConfigChange` | Un fichier de config change |
| `CwdChanged` | Le répertoire de travail change |
| `InstructionsLoaded` | Un fichier CLAUDE.md ou une règle est chargé |

---

## Les matchers — cibler un outil précis

Le `matcher` détermine pour quel outil le hook se déclenche :

| Pattern | Évaluation | Exemple |
|---------|-----------|---------|
| `""` ou omis | Tous les outils | Se déclenche à chaque événement |
| Lettres, chiffres, `_`, `\|` | Correspondance exacte ou liste | `"Bash"`, `"Edit\|Write"` |
| Contient d'autres caractères | Regex JavaScript | `"^Notebook"`, `"mcp__memory__.*"` |

```json
"matcher": "Edit|Write"         // Edit OU Write
"matcher": "Bash"               // uniquement Bash
"matcher": "mcp__memory__.*"    // tous les outils du serveur MCP "memory"
"matcher": ""                   // tous les outils sans exception
```

---

## Exemples pratiques

### 1. Notification macOS quand Claude attend ton input

```json
{
  "hooks": {
    "Notification": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "osascript -e 'display notification \"Claude Code a besoin de toi\" with title \"Claude Code\"'"
          }
        ]
      }
    ]
  }
}
```

Équivalents Linux / Windows :
```bash
# Linux
"command": "notify-send 'Claude Code' 'En attente de ton input'"

# Windows PowerShell
"command": "powershell.exe -Command \"[System.Windows.Forms.MessageBox]::Show('Claude Code attend', 'Claude Code')\""
```

---

### 2. Auto-format avec Prettier après chaque édition

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "jq -r '.tool_input.file_path' | xargs npx prettier --write"
          }
        ]
      }
    ]
  }
}
```

> `jq` est utilisé pour extraire le chemin du fichier depuis le JSON reçu sur stdin. Installer avec `brew install jq` (macOS) ou `apt install jq` (Linux).

---

### 3. Bloquer les modifications de fichiers sensibles

Créer `.claude/hooks/protect-files.sh` :

```bash
#!/bin/bash
# protect-files.sh

INPUT=$(cat)
FILE_PATH=$(echo "$INPUT" | jq -r '.tool_input.file_path // empty')

PROTECTED_PATTERNS=(".env" "package-lock.json" ".git/")

for pattern in "${PROTECTED_PATTERNS[@]}"; do
  if [[ "$FILE_PATH" == *"$pattern"* ]]; then
    echo "Bloqué : $FILE_PATH correspond au pattern protégé '$pattern'" >&2
    exit 2   # ← code 2 = bloquant
  fi
done

exit 0
```

```bash
chmod +x .claude/hooks/protect-files.sh
```

Puis dans `.claude/settings.json` :

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": ".claude/hooks/protect-files.sh"
          }
        ]
      }
    ]
  }
}
```

---

### 4. Bloquer une commande Bash dangereuse

```bash
#!/bin/bash
COMMAND=$(cat | jq -r '.tool_input.command')

if echo "$COMMAND" | grep -q 'rm -rf'; then
  echo "Commande destructive bloquée" >&2
  exit 2
fi

exit 0
```

---

### 5. Injecter des variables d'environnement au démarrage de session

```bash
#!/bin/bash
# SessionStart hook — persiste des variables pour toute la session

if [ -n "$CLAUDE_ENV_FILE" ]; then
  echo 'export NODE_ENV=development' >> "$CLAUDE_ENV_FILE"
  echo 'export DEBUG=true' >> "$CLAUDE_ENV_FILE"
fi

exit 0
```

---

## Illustration : flux PreToolUse avec hook bloquant

```
Claude veut exécuter : Edit("/etc/hosts")
            │
            ▼
    Hook PreToolUse s'exécute
    → script reçoit le JSON sur stdin
    → vérifie le chemin du fichier
            │
     ┌──────┴──────┐
     │             │
  exit 2        exit 0
  (bloquant)    (succès)
     │             │
     ▼             ▼
Claude reçoit  L'outil s'exécute
le message     normalement
d'erreur et
s'adapte
```

---

## Codes de sortie

| Code | Comportement |
|------|-------------|
| `0` | Succès — lire stdout pour les décisions JSON |
| `2` | Erreur **bloquante** — stderr devient le message d'erreur affiché à Claude |
| Autre | Erreur non bloquante — stderr dans les logs debug uniquement |

---

## Format de sortie JSON (optionnel)

Un hook peut retourner un JSON sur stdout pour influencer le comportement de Claude Code :

```json
{
  "continue": true,
  "suppressOutput": false,
  "systemMessage": "Message d'avertissement affiché à Claude",
  "additionalContext": "Contexte supplémentaire injecté dans la conversation",
  "hookSpecificOutput": {
    "hookEventName": "PreToolUse",
    "permissionDecision": "allow|deny|ask|defer",
    "permissionDecisionReason": "Pourquoi cette décision"
  }
}
```

Cas d'usage : un hook `PreToolUse` peut retourner `"permissionDecision": "allow"` pour auto-approuver un outil, ou `"deny"` pour le bloquer avec une raison.

---

## Champs de configuration d'un hook

```json
{
  "type": "command",
  "command": "/chemin/script.sh",
  "args": [],
  "async": false,
  "timeout": 60,
  "statusMessage": "Vérification en cours...",
  "shell": "bash",
  "if": "Bash(git *)",
  "once": false
}
```

| Champ | Description |
|-------|-------------|
| `type` | `command`, `http`, `mcp_tool`, `prompt`, `agent` |
| `command` | Commande shell ou chemin vers un script |
| `args` | Liste d'arguments (passe en mode exec sans shell si présent) |
| `async` | `true` = s'exécute en arrière-plan sans bloquer Claude |
| `timeout` | Secondes avant annulation (défaut : 60) |
| `statusMessage` | Message affiché dans le spinner pendant l'exécution |
| `shell` | `"bash"` ou `"powershell"` |
| `if` | Filtre par règle de permission (ex: `"Bash(rm *)"`) |
| `once` | `true` = s'exécute une seule fois par session puis se supprime |

---

## Variables d'environnement disponibles dans les hooks

| Variable | Description |
|----------|-------------|
| `CLAUDE_PROJECT_DIR` | Racine du projet |
| `CLAUDE_PLUGIN_ROOT` | Dossier d'installation du plugin |
| `CLAUDE_PLUGIN_DATA` | Données persistantes du plugin |
| `CLAUDE_EFFORT` | Niveau d'effort actif |
| `CLAUDE_ENV_FILE` | Chemin pour persister des variables d'env entre les commandes Bash |
| `CLAUDE_CODE_REMOTE` | `"true"` dans les environnements distants |

---

## Exec form vs Shell form

Deux façons d'écrire la commande d'un hook :

```json
// Shell form (défaut, sans "args") — le shell interprète la commande
{
  "command": "jq -r '.tool_input.file_path' | xargs prettier --write"
}

// Exec form (avec "args") — pas d'interprétation shell, les placeholders sont sûrs
{
  "command": "node",
  "args": ["${CLAUDE_PLUGIN_ROOT}/scripts/format.js", "--fix"]
}
```

> Préférer **exec form** quand le script utilise des variables de chemin (`${CLAUDE_PLUGIN_ROOT}`, etc.) — les chemins avec espaces ne cassent pas.

---

## Désactiver tous les hooks

```json
{
  "disableAllHooks": true
}
```

---

## Points clés à retenir

- Un hook s'exécute **toujours** au bon moment — pas de risque qu'il soit "oublié" par Claude.
- `PreToolUse` + `exit 2` = bloquer un outil avec un message d'erreur pour Claude.
- `PostToolUse` = formater, logger, notifier après une action.
- `Notification` = alerter quand Claude attend ton input.
- Le `matcher` `"Edit|Write"` cible les deux outils à la fois.
- `async: true` pour les hooks longs qui ne doivent pas bloquer Claude.
- `/hooks` dans une session pour voir tous les hooks actifs et leur source.

---

## Qu'est-ce que YAML et JSON ici ?

> Les hooks se configurent en **JSON** (dans `settings.json`), pas en YAML. JSON est un format de données structurées avec des accolades `{}`, des crochets `[]` et des guillemets doubles obligatoires — c'est le format natif de la majorité des fichiers de configuration d'outils JavaScript/Node.
>
> ```json
> {
>   "hooks": {
>     "PostToolUse": [
>       {
>         "matcher": "Edit",
>         "hooks": [{ "type": "command", "command": "prettier --write" }]
>       }
>     ]
>   }
> }
> ```
>
> Le **YAML** (utilisé dans les frontmatters des skills) et le **JSON** sont deux formats de sérialisation différents. JSON est plus strict (pas de commentaires, guillemets obligatoires). YAML est plus lisible mais plus fragile (l'indentation compte).

---

## 📚 Sources

- [Hooks — Guide pratique Claude Code](https://code.claude.com/docs/en/hooks-guide)
- [Hooks — Référence complète](https://code.claude.com/docs/en/hooks)
- [Permissions et hooks](https://code.claude.com/docs/en/permissions)
- [Skills — Hooks dans les skills](https://code.claude.com/docs/en/skills)
