# Le MCP dans Claude Code

## C'est quoi le MCP ?

Le **Model Context Protocol (MCP)** est un standard ouvert qui permet à Claude Code de se connecter à des outils et services externes — bases de données, APIs, outils d'équipe, etc. — et d'interagir avec eux directement depuis une session.

```
Sans MCP : tu copies/colles des données depuis d'autres outils dans le chat
Avec MCP : Claude lit et agit sur ces systèmes directement
```

Exemples de ce que Claude peut faire avec des serveurs MCP :

- "Implémente la feature décrite dans JIRA ENG-4521 et crée une PR sur GitHub"
- "Consulte Sentry pour voir les erreurs des dernières 24h"
- "Trouve les 10 derniers utilisateurs en base PostgreSQL qui n'ont pas acheté depuis 90 jours"
- "Mets à jour le template d'email selon les nouvelles maquettes Figma partagées sur Slack"

---

## Les trois types de serveurs MCP

| Type                  | Transport          | Utilisation                    |
| --------------------- | ------------------ | ------------------------------ |
| **HTTP** (recommandé) | Requêtes HTTP      | Services cloud, APIs distantes |
| **SSE**               | Server-Sent Events | Déprécié — préférer HTTP       |
| **stdio**             | Processus local    | Scripts locaux, outils système |

---

## Ajouter un serveur MCP

### Serveur HTTP (cloud)

```bash
# Syntaxe de base
claude mcp add --transport http <nom> <url>

# Exemples concrets
claude mcp add --transport http notion https://mcp.notion.com/mcp
claude mcp add --transport http sentry https://mcp.sentry.dev/mcp
claude mcp add --transport http github https://api.githubcopilot.com/mcp/ \
  --header "Authorization: Bearer TON_TOKEN_GITHUB"
```

### Serveur stdio (local)

```bash
# Syntaxe de base
claude mcp add <nom> -- <commande> [args...]

# Exemples
claude mcp add --transport stdio airtable \
  --env AIRTABLE_API_KEY=ta_cle \
  -- npx -y airtable-mcp-server

claude mcp add --transport stdio db \
  -- npx -y @bytebase/dbhub \
  --dsn "postgresql://readonly:pass@localhost:5432/mydb"
```

> **Ordre important :** toutes les options (`--transport`, `--env`, `--scope`, `--header`) doivent venir **avant** le nom du serveur. Le `--` sépare le nom du serveur de la commande.

### Depuis un JSON

```bash
# Ajouter depuis une config JSON directe
claude mcp add-json weather '{"type":"http","url":"https://api.weather.com/mcp"}'

# Avec authentification
claude mcp add-json github \
  '{"type":"http","url":"https://api.githubcopilot.com/mcp/","headers":{"Authorization":"Bearer TON_TOKEN"}}'
```

### Importer depuis Claude Desktop

```bash
claude mcp add-from-claude-desktop
# Interface interactive pour choisir quels serveurs importer
# (macOS et WSL uniquement)
```

---

## Gérer ses serveurs MCP

```bash
# Lister tous les serveurs configurés
claude mcp list

# Détails d'un serveur
claude mcp get github

# Supprimer un serveur
claude mcp remove github
```

Dans une session interactive :

```
/mcp   ← voir le statut, authentifier, gérer les serveurs
```

---

## Les scopes — où sont stockés les serveurs

| Scope            | Portée                    | Partagé       | Stocké dans             |
| ---------------- | ------------------------- | ------------- | ----------------------- |
| `local` (défaut) | Projet courant uniquement | Non           | `~/.claude.json`        |
| `project`        | Projet courant uniquement | Oui (via Git) | `.mcp.json` à la racine |
| `user`           | Tous tes projets          | Non           | `~/.claude.json`        |

```bash
# Local (défaut, perso, ce projet)
claude mcp add --transport http stripe https://mcp.stripe.com

# Projet (partagé avec l'équipe via Git)
claude mcp add --transport http paypal --scope project https://mcp.paypal.com/mcp

# Utilisateur (tous tes projets)
claude mcp add --transport http hubspot --scope user https://mcp.hubspot.com/anthropic
```

---

## Le fichier `.mcp.json` (scope projet)

Quand tu ajoutes un serveur avec `--scope project`, Claude Code crée automatiquement un fichier `.mcp.json` à la racine du projet — à committer dans Git pour le partager avec l'équipe.

```json
{
  "mcpServers": {
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp/",
      "headers": {
        "Authorization": "Bearer ${GITHUB_TOKEN}"
      }
    },
    "db-locale": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@bytebase/dbhub", "--dsn", "${DATABASE_URL}"],
      "env": {
        "CACHE_DIR": "/tmp"
      }
    }
  }
}
```

### Variables d'environnement dans `.mcp.json`

Les valeurs sensibles (tokens, URLs) ne doivent pas être en clair dans le fichier. Utiliser la syntaxe `${VARIABLE}` :

```json
{
  "mcpServers": {
    "api": {
      "type": "http",
      "url": "${API_BASE_URL:-https://api.example.com}/mcp",
      "headers": {
        "Authorization": "Bearer ${API_KEY}"
      }
    }
  }
}
```

| Syntaxe          | Comportement                               |
| ---------------- | ------------------------------------------ |
| `${VAR}`         | Valeur de la variable d'env `VAR`          |
| `${VAR:-defaut}` | Valeur de `VAR` si définie, sinon `defaut` |

---

## Authentification OAuth

Beaucoup de serveurs cloud (Sentry, Notion, Slack…) utilisent OAuth. Claude Code le gère automatiquement.

```bash
# 1. Ajouter le serveur
claude mcp add --transport http sentry https://mcp.sentry.dev/mcp

# 2. Lancer l'authentification dans une session
/mcp
# → suivre les étapes dans le navigateur
```

Les tokens OAuth sont stockés de façon sécurisée et rafraîchis automatiquement.

### Avec des credentials OAuth pré-configurés

Pour les serveurs qui ne supportent pas l'enregistrement dynamique :

```bash
claude mcp add --transport http \
  --client-id ton-client-id --client-secret --callback-port 8080 \
  mon-serveur https://mcp.example.com/mcp
```

---

## Illustration : flux d'une requête MCP

```
Tu demandes : "Trouve les erreurs Sentry des dernières 24h"
            │
            ▼
    Claude identifie qu'il a besoin
    des outils du serveur MCP "sentry"
            │
            ▼
    Claude appelle l'outil MCP
    (ex: sentry_list_errors avec filter=24h)
            │
            ▼
    Le serveur MCP interroge l'API Sentry
    et retourne les données JSON
            │
            ▼
    Claude analyse les résultats
    et te répond en langage naturel
```

---

## Référencer des ressources MCP avec `@`

Les serveurs MCP peuvent exposer des **ressources** (fichiers, données) accessibles via `@` comme les fichiers locaux :

```
@github:issue://123           ← une issue GitHub
@docs:file://api/auth         ← un fichier de doc
@postgres:schema://users      ← le schéma d'une table
```

Exemples d'utilisation dans le chat :

```
"Analyse @github:issue://456 et propose un correctif"
"Compare @postgres:schema://users avec @docs:file://database/user-model"
```

---

## Utiliser les prompts MCP comme commandes slash

Les serveurs MCP peuvent exposer des prompts qui deviennent des commandes `/` dans Claude Code :

```
/mcp__github__list_prs              ← lister les PRs
/mcp__github__pr_review 456         ← revoir la PR 456
/mcp__jira__create_issue "Bug" high ← créer une issue Jira
```

Format : `/mcp__<nom-serveur>__<nom-prompt> [arguments]`

---

## MCP Tool Search — économiser le contexte

Par défaut, Claude Code ne charge que les **noms** des outils MCP au démarrage. Il recherche les définitions complètes à la demande avec `ToolSearch`. Cela évite de saturer la fenêtre de contexte quand tu as beaucoup de serveurs.

```bash
# Désactiver le tool search (charge tout au démarrage)
ENABLE_TOOL_SEARCH=false claude

# Mode seuil : charge si < 5% du contexte, diffère sinon
ENABLE_TOOL_SEARCH=auto:5 claude
```

Pour forcer un serveur à toujours charger ses outils au démarrage :

```json
{
  "mcpServers": {
    "outils-essentiels": {
      "type": "http",
      "url": "https://mcp.example.com",
      "alwaysLoad": true
    }
  }
}
```

---

## Exemples de serveurs populaires

### GitHub

```bash
claude mcp add --transport http github https://api.githubcopilot.com/mcp/ \
  --header "Authorization: Bearer TON_PAT_GITHUB"
```

```
"Revue la PR #456 et suggère des améliorations"
"Crée une issue pour le bug qu'on vient de trouver"
```

### Sentry (monitoring)

```bash
claude mcp add --transport http sentry https://mcp.sentry.dev/mcp
# puis /mcp pour s'authentifier
```

```
"Quelles sont les erreurs les plus fréquentes depuis hier ?"
"Quel déploiement a introduit ces nouvelles erreurs ?"
```

### PostgreSQL

```bash
claude mcp add --transport stdio db \
  -- npx -y @bytebase/dbhub \
  --dsn "postgresql://readonly:pass@localhost:5432/mydb"
```

```
"Quel est le chiffre d'affaires ce mois-ci ?"
"Montre-moi le schéma de la table orders"
```

---

## Utiliser Claude Code lui-même comme serveur MCP

Claude Code peut être exposé en tant que serveur MCP pour d'autres applications (ex: Claude Desktop) :

```bash
claude mcp serve
```

Configuration dans `claude_desktop_config.json` :

```json
{
  "mcpServers": {
    "claude-code": {
      "type": "stdio",
      "command": "/chemin/vers/claude",
      "args": ["mcp", "serve"]
    }
  }
}
```

---

## Variables d'environnement utiles

| Variable                      | Description                                                       |
| ----------------------------- | ----------------------------------------------------------------- |
| `MCP_TIMEOUT`                 | Timeout de démarrage des serveurs en ms (ex: `MCP_TIMEOUT=10000`) |
| `MAX_MCP_OUTPUT_TOKENS`       | Limite de tokens pour la sortie des outils MCP (défaut : 25 000)  |
| `ENABLE_TOOL_SEARCH`          | Contrôle le tool search (`true`, `false`, `auto`, `auto:N`)       |
| `ENABLE_CLAUDEAI_MCP_SERVERS` | `false` pour désactiver les serveurs claude.ai                    |
| `CLAUDE_PROJECT_DIR`          | Racine du projet, disponible dans les serveurs stdio              |

---

## Points clés à retenir

- MCP = standard ouvert pour connecter Claude à des outils externes.
- Trois transports : **HTTP** (recommandé), SSE (déprécié), **stdio** (local).
- Trois scopes : **local** (perso, ce projet), **project** (équipe via `.mcp.json`), **user** (tous tes projets).
- Le fichier `.mcp.json` se committe dans Git pour partager les serveurs avec l'équipe.
- Ne jamais écrire les tokens en clair — utiliser `${MA_VARIABLE}` dans `.mcp.json`.
- L'authentification OAuth se gère via `/mcp` dans une session.
- `@serveur:protocole://ressource` pour référencer des données MCP dans le chat.
- Le Tool Search est actif par défaut — les outils se chargent à la demande pour économiser le contexte.

---

## Qu'est-ce que le protocole MCP exactement ?

> **MCP (Model Context Protocol)** est un standard ouvert créé par Anthropic, similaire à ce que USB représente pour les périphériques : un connecteur universel entre les LLMs et les outils externes.
>
> Avant MCP, chaque outil devait être intégré spécifiquement pour chaque IA. MCP définit une interface commune : n'importe quel outil qui implémente le protocole peut être connecté à n'importe quel LLM qui le supporte (Claude, mais aussi d'autres).
>
> Un **serveur MCP** est un programme (local ou distant) qui expose des **outils** (fonctions appelables), des **ressources** (données lisibles) et des **prompts** (templates de commandes) via ce protocole standardisé.
>
> ```
> Claude (client MCP) ←──── protocole MCP ────→ Serveur MCP
>                                                (GitHub, Sentry,
>                                                 ta propre API...)
> ```

---

## 📚 Sources

- [MCP — Documentation Claude Code](https://code.claude.com/docs/en/mcp)
- [MCP — Site officiel du protocole](https://modelcontextprotocol.io/introduction)
- [Anthropic Directory — serveurs MCP disponibles](https://claude.ai/directory)
- [Managed MCP — configuration centralisée](https://code.claude.com/docs/en/managed-mcp)
- [Channels — pousser des événements dans Claude](https://code.claude.com/docs/en/channels)
- [Context7 — recherche sur internet consommant moins de token](https://context7.com/)
- [Exa — recherche sur internet consommant moins de token - payant](https://exa.ai/)
