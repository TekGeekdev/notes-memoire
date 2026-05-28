# Les Skills dans Claude Code

## C'est quoi un skill ?

Un **skill** est un fichier d'instructions personnalisées qui étend les capacités de Claude Code. Tu crées un fichier `SKILL.md` et Claude l'ajoute à sa boîte à outils — il peut s'en servir automatiquement quand c'est pertinent, ou tu peux l'invoquer directement avec `/nom-du-skill`.

```
SKILL.md  →  commande /nom-du-skill + chargement automatique par Claude
```

> **Quand créer un skill ?**
>
> - Tu colles les mêmes instructions dans le chat à chaque session
> - Une section de ton `CLAUDE.md` est devenue une procédure plutôt qu'un fait
> - Tu veux encapsuler un workflow réutilisable (deploy, commit, review...)
>
> Contrairement à `CLAUDE.md`, le contenu d'un skill ne se charge **que lorsqu'il est utilisé** — les longues références ne coûtent rien en tokens tant qu'elles ne sont pas invoquées.

---

## Skills vs Commandes : même chose

Les anciennes "custom commands" (fichiers dans `.claude/commands/`) et les skills (fichiers dans `.claude/skills/`) fonctionnent **exactement pareil**. Les commandes continuent de marcher, mais les skills ajoutent des fonctionnalités supplémentaires (dossier de fichiers support, frontmatter avancé, invocation par Claude).

---

## Les skills bundlés (intégrés)

Claude Code inclut des skills prêts à l'emploi, accessibles dans toutes les sessions :

| Skill                  | Description                                                       |
| ---------------------- | ----------------------------------------------------------------- |
| `/code-review`         | Revue du diff actuel pour des bugs de logique                     |
| `/security-review`     | Analyse les changements pour des failles de sécurité              |
| `/debug`               | Active les logs debug et aide à diagnostiquer                     |
| `/batch`               | Orchestration de changements massifs en parallèle sur le codebase |
| `/loop`                | Répète un prompt en boucle à intervalles réguliers                |
| `/claude-api`          | Charge la référence API Anthropic pour ton langage                |
| `/run`                 | Lance l'app et vérifie un changement dans l'app en direct         |
| `/verify`              | Build et lance l'app pour confirmer qu'un changement fonctionne   |
| `/run-skill-generator` | Apprend à `/run` et `/verify` comment lancer ton projet           |

---

## Structure d'un skill

Chaque skill est un **dossier** contenant au minimum un fichier `SKILL.md` :

```
mon-skill/
├── SKILL.md          ← instructions principales (obligatoire)
├── reference.md      ← doc de référence chargée à la demande
├── examples.md       ← exemples de sortie attendue
└── scripts/
    └── helper.py     ← script que Claude peut exécuter
```

Le nom du **dossier** devient la commande slash : `mon-skill/` → `/mon-skill`.

---

## Où placer ses skills

| Niveau     | Chemin                            | Portée                    |
| ---------- | --------------------------------- | ------------------------- |
| Entreprise | Via managed settings              | Tous les utilisateurs     |
| Personnel  | `~/.claude/skills/<nom>/SKILL.md` | Tous tes projets          |
| Projet     | `.claude/skills/<nom>/SKILL.md`   | Ce projet uniquement      |
| Plugin     | `<plugin>/skills/<nom>/SKILL.md`  | Là où le plugin est actif |

En cas de conflit de nom, l'ordre de priorité est : entreprise > personnel > projet.

---

## Créer son premier skill — exemple complet

### Cas : résumer les changements Git non commités

```bash
# 1. Créer le dossier du skill (personnel, disponible partout)
mkdir -p ~/.claude/skills/resume-changements
```

```yaml
# 2. Créer ~/.claude/skills/resume-changements/SKILL.md

---
description: Résume les changements non commités et signale les risques. Utiliser quand l'utilisateur demande ce qui a changé, veut un message de commit, ou demande à revoir son diff.
---

## Changements actuels

!`git diff HEAD`

## Instructions

Résume les changements ci-dessus en 2-3 points, puis liste les risques détectés
(gestion d'erreur manquante, valeurs codées en dur, tests à mettre à jour).
Si le diff est vide, dis qu'il n'y a pas de changements non commités.
```

```bash
# 3. Tester dans une session Claude Code
/resume-changements

# Ou laisser Claude le détecter automatiquement :
"Qu'est-ce que j'ai changé ?"
```

> La ligne `` !`git diff HEAD` `` est une **injection de contexte dynamique** : Claude Code exécute la commande et insère sa sortie dans le prompt avant que Claude ne le lise.

---

## Le frontmatter — configuration du skill

Le frontmatter est un **bloc en syntaxe YAML placé tout en haut du fichier `SKILL.md`**, encadré par des `---`. Ce n'est **pas un fichier séparé** — tout reste dans le même `.md`. Les autres fichiers du dossier (reference.md, examples.md…) sont de simples Markdown sans frontmatter.

```
SKILL.md
├── ---  ← début du frontmatter (YAML)
│   name: mon-skill
│   description: ...
├── ---  ← fin du frontmatter
└── ## Suite du fichier en Markdown normal
```

Tous les champs sont **optionnels**. Un `SKILL.md` sans frontmatter fonctionne — mais sans `description`, Claude n'a aucun indice pour charger le skill automatiquement, il faudra toujours l'invoquer manuellement avec `/nom-du-skill`.

```yaml
---
name: mon-skill
description: Ce que fait le skill et quand l'utiliser
when_to_use: Phrases déclencheurs supplémentaires, exemples de demandes
argument-hint: [numéro-issue]
arguments: [issue, branche]
disable-model-invocation: true
user-invocable: false
allowed-tools: Bash(git *) Read
model: claude-opus-4-7
effort: high
context: fork
agent: Explore
paths:
  - 'src/api/**/*.ts'
---
```

### Référence des champs

| Champ                      | Description                                                           |
| -------------------------- | --------------------------------------------------------------------- |
| `name`                     | Nom affiché. Par défaut : nom du dossier                              |
| `description`              | Ce que fait le skill — Claude l'utilise pour décider quand le charger |
| `when_to_use`              | Contexte supplémentaire de déclenchement                              |
| `argument-hint`            | Indication affichée dans l'autocomplétion (ex: `[numéro]`)            |
| `arguments`                | Noms des arguments positionnels pour la substitution `$nom`           |
| `disable-model-invocation` | `true` = seulement toi peux l'invoquer, pas Claude                    |
| `user-invocable`           | `false` = caché du menu `/`, seul Claude peut l'invoquer              |
| `allowed-tools`            | Outils utilisables sans confirmation quand le skill est actif         |
| `model`                    | Modèle à utiliser pour ce skill                                       |
| `effort`                   | Niveau d'effort (`low`, `medium`, `high`, `xhigh`, `max`)             |
| `context`                  | `fork` = s'exécute dans un sous-agent isolé                           |
| `agent`                    | Type de sous-agent si `context: fork` (`Explore`, `Plan`, etc.)       |
| `paths`                    | Glob patterns — le skill se charge uniquement pour ces fichiers       |

---

## Qui peut invoquer un skill ?

```
┌────────────────────────────────────────────────────────────┐
│             Contrôle d'invocation du skill                 │
├──────────────────────────┬─────────────┬───────────────────┤
│ Frontmatter              │ Toi (/)     │ Claude (auto)     │
├──────────────────────────┼─────────────┼───────────────────┤
│ (défaut)                 │ Oui         │ Oui               │
│ disable-model-invocation │ Oui         │ Non               │
│ user-invocable: false    │ Non         │ Oui               │
└──────────────────────────┴─────────────┴───────────────────┘
```

**Utiliser `disable-model-invocation: true`** pour les workflows à effets de bord que tu veux contrôler manuellement : `/deploy`, `/commit`, `/send-slack-message` — tu ne veux pas que Claude décide de déployer parce que le code lui semble prêt.

**Utiliser `user-invocable: false`** pour les connaissances de fond non actionnables directement : un skill `contexte-systeme-legacy` que Claude charge quand c'est pertinent, mais qui n'a pas de sens en tant que commande `/`.

---

## Passer des arguments à un skill

Les arguments sont accessibles via `$ARGUMENTS` ou par index `$0`, `$1`, etc.

### Exemple — fixer une issue GitHub

```yaml
---
name: fix-issue
description: Corrige une issue GitHub par son numéro
disable-model-invocation: true
---
Corrige l'issue GitHub $ARGUMENTS en suivant nos standards de code.

1. Lire la description de l'issue
2. Comprendre les exigences
3. Implémenter le correctif
4. Écrire les tests
5. Créer un commit
```

Invocation : `/fix-issue 123` → Claude reçoit "Corrige l'issue GitHub 123..."

### Exemple — migration de composant (arguments nommés)

```yaml
---
name: migrer-composant
description: Migre un composant d'un framework à un autre
arguments: [composant, source, cible]
---
Migre le composant $composant de $source vers $cible.
Conserve tous les comportements et tests existants.
```

Invocation : `/migrer-composant SearchBar React Vue`

### Variables disponibles

| Variable               | Description                                |
| ---------------------- | ------------------------------------------ |
| `$ARGUMENTS`           | Tous les arguments passés à l'invocation   |
| `$ARGUMENTS[0]`, `$0`  | Premier argument                           |
| `$ARGUMENTS[1]`, `$1`  | Deuxième argument                          |
| `$nom`                 | Argument nommé (déclaré dans `arguments:`) |
| `${CLAUDE_SESSION_ID}` | ID de la session courante                  |
| `${CLAUDE_EFFORT}`     | Niveau d'effort actif                      |
| `${CLAUDE_SKILL_DIR}`  | Chemin absolu du dossier du skill          |

---

## Injection de contexte dynamique

La syntaxe `` !`commande` `` exécute un shell **avant** que Claude ne lise le skill. La sortie remplace le placeholder — Claude reçoit les données réelles, pas la commande.

```yaml
---
name: resume-pr
description: Résume les changements d'une pull request
---

## Contexte de la PR

- Diff : !`gh pr diff`
- Commentaires : !`gh pr view --comments`
- Fichiers modifiés : !`gh pr diff --name-only`

## Ta tâche

Résume cette pull request en 3-5 points clés...
```

Pour des commandes multi-lignes, utiliser un bloc de code ouvert avec ` ```! ` :

````markdown
## Environnement

```!
node --version
npm --version
git status --short
```
````

> C'est du **prétraitement**, pas quelque chose que Claude exécute. Claude ne voit que le résultat final inséré dans le prompt.

---

## Exécuter un skill dans un sous-agent

Ajouter `context: fork` pour isoler le skill dans un sous-agent séparé. Le contenu du `SKILL.md` devient le prompt du sous-agent, sans accès à l'historique de conversation.

```yaml
---
name: recherche-approfondie
description: Recherche un sujet en profondeur dans le codebase
context: fork
agent: Explore
---

Recherche $ARGUMENTS dans le codebase :

1. Trouver les fichiers pertinents avec Glob et Grep
2. Lire et analyser le code
3. Résumer les résultats avec des références précises aux fichiers
```

| Approche                          | Contexte de départ      | Charge              |
| --------------------------------- | ----------------------- | ------------------- |
| Skill avec `context: fork`        | Vide (pas d'historique) | Contenu du SKILL.md |
| Sous-agent avec skills préchargés | Vide                    | Skills + CLAUDE.md  |

---

## Pré-approuver des outils pour un skill

Le champ `allowed-tools` permet à Claude d'utiliser certains outils sans demander de confirmation pendant l'exécution du skill :

```yaml
---
name: commit
description: Stager et commiter les changements courants
disable-model-invocation: true
allowed-tools: Bash(git add *) Bash(git commit *) Bash(git status *)
---
Stager tous les changements et créer un commit avec un message descriptif
au format Conventional Commits.
```

---

## Illustration : cycle de vie d'un skill

```
Tu tapes : /mon-skill argument

        │
        ▼
Claude Code exécute les commandes !`...`
et remplace les placeholders $ARGUMENTS
        │
        ▼
Le contenu rendu entre dans la conversation
comme un message unique
        │
        ▼
Claude suit les instructions du skill
en utilisant ses outils habituels
        │
        ▼
Le contenu reste dans le contexte
pour le reste de la session
(recompacté si besoin lors du /compact)
```

---

## Contrôler les skills depuis les settings

Le champ `skillOverrides` dans `settings.json` contrôle la visibilité sans modifier le SKILL.md :

```json
{
  "skillOverrides": {
    "contexte-legacy": "name-only",
    "deploy": "off"
  }
}
```

| Valeur                  | Listé à Claude    | Dans le menu `/` |
| ----------------------- | ----------------- | ---------------- |
| `"on"` (défaut)         | Nom + description | Oui              |
| `"name-only"`           | Nom seul          | Oui              |
| `"user-invocable-only"` | Caché             | Oui              |
| `"off"`                 | Caché             | Non              |

Depuis une session : `/skills` → sélectionner un skill → `Espace` pour cycler l'état → `Entrée` pour sauvegarder.

---

## Restreindre l'accès de Claude aux skills

Via les permissions :

```json
{
  "permissions": {
    "deny": ["Skill"],
    "allow": ["Skill(commit)", "Skill(review-pr *)"]
  }
}
```

| Règle             | Effet                                                   |
| ----------------- | ------------------------------------------------------- |
| `Skill`           | Désactive tous les skills                               |
| `Skill(deploy)`   | Bloque uniquement `/deploy`                             |
| `Skill(deploy *)` | Bloque `/deploy` et toutes ses variantes avec arguments |

---

## Bonnes pratiques

- **Garder `SKILL.md` sous 500 lignes** — déplacer la référence détaillée dans des fichiers support
- **`description` en premier** — écrire le cas d'usage principal en premier, la description est tronquée à 1 536 caractères dans le listing
- **`disable-model-invocation: true`** pour tout ce qui a des effets de bord (deploy, envoi de messages, commits)
- **`${CLAUDE_SKILL_DIR}`** pour référencer les scripts bundlés — fonctionne quel que soit le niveau d'installation du skill
- **Rechargement à chaud** — modifier un `SKILL.md` pendant une session prend effet immédiatement, pas besoin de redémarrer

---

## Résolution des problèmes courants

**Le skill ne se déclenche pas automatiquement**

- Vérifier que la `description` contient les mots-clés que l'utilisateur dirait naturellement
- Taper `Quels skills sont disponibles ?` pour vérifier qu'il apparaît
- L'invoquer directement avec `/nom-du-skill` pour tester

**Le skill se déclenche trop souvent**

- Rendre la `description` plus spécifique
- Ajouter `disable-model-invocation: true`

**La description est tronquée**

- Mettre le cas d'usage principal en tout premier dans `description`
- Utiliser `skillOverrides: "name-only"` pour les skills peu utilisés
- Augmenter le budget via `skillListingBudgetFraction` dans settings

---

## Points clés à retenir

- Un skill = un dossier avec un `SKILL.md` → crée automatiquement une commande `/nom`
- Le contenu ne se charge **que lors de l'utilisation** — pas de coût en tokens tant qu'inactif
- `` !`commande` `` injecte la sortie shell dans le prompt avant que Claude ne lise le skill
- `disable-model-invocation: true` pour les workflows à effets de bord (deploy, commit...)
- `context: fork` pour exécuter dans un sous-agent isolé sans historique
- `allowed-tools` pré-approuve des outils pour éviter les confirmations pendant le skill
- Les skills se rechargent à chaud — pas besoin de redémarrer après une modification

---

## Qu'est-ce qu'un frontmatter ?

> Un **frontmatter** est un bloc de métadonnées placé **tout en haut** d'un fichier texte, encadré par deux lignes de `---`. Il est écrit en **YAML** (un format clé-valeur lisible par les humains) et sert à configurer le comportement du fichier sans mélanger la configuration avec le contenu.
>
> Ce format est très répandu : Jekyll, Hugo, Obsidian, Notion et beaucoup d'outils de documentation l'utilisent pour stocker des paramètres (titre, date, tags, etc.) séparément du corps du document.
>
> ```yaml
> ---           ← délimiteur d'ouverture
> name: mon-skill
> description: Ce que fait le skill
> ---           ← délimiteur de fermeture
>
> ## Contenu Markdown normal ici...
> ```
>
> Tout ce qui est entre les deux `---` est du YAML (la config). Tout ce qui vient après est du Markdown (le contenu). Le fichier reste un `.md` — le frontmatter n'est pas un fichier séparé.

---

## 📚 Sources

- [Skills — Documentation officielle Claude Code](https://code.claude.com/docs/en/skills)
- [Commandes slash et skills bundlés](https://code.claude.com/docs/en/commands)
- [Sous-agents et skills préchargés](https://code.claude.com/docs/en/sub-agents)
- [Permissions — contrôle d'accès aux skills](https://code.claude.com/docs/en/permissions)
- [Hooks — automatiser les workflows](https://code.claude.com/docs/en/hooks-guide)
- [skills sh - biblio de skills](https://www.skills.sh/)
