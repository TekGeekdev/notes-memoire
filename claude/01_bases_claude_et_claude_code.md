# Les bases de Claude et Claude Code

## Qu'est-ce que Claude ?

Claude est un **assistant IA** (intelligence artificielle) développé par **Anthropic**. Il est basé sur un **LLM** (*Large Language Model* — grand modèle de langage), c'est-à-dire un modèle entraîné sur d'énormes quantités de texte pour comprendre et générer du langage naturel.

Claude peut :

- Répondre à des questions complexes
- Écrire, analyser et expliquer du code
- Rédiger, résumer et reformuler du texte
- Raisonner étape par étape sur des problèmes
- Utiliser des **outils** (fichiers, recherche web, APIs, etc.) selon la configuration

### Les familles de modèles Claude (2025)

Anthropic organise ses modèles en **trois niveaux** selon la puissance, la vitesse et le coût.

| Modèle | Identifiant API | Vitesse | Coût | Usage recommandé |
|--------|----------------|---------|------|-----------------|
| **Opus 4** | `claude-opus-4-7` | Lent | Élevé | Raisonnement complexe, analyse approfondie |
| **Sonnet 4** | `claude-sonnet-4-6` | Moyen | Moyen | Usage général, développement, écriture |
| **Haiku 4** | `claude-haiku-4-5-20251001` | Rapide | Faible | Tâches simples, systèmes haute fréquence |

> **Règle de base :** par défaut, utiliser **Sonnet** pour la plupart des projets. Passer à **Opus** pour les raisonnements profonds et **Haiku** pour les tâches répétitives à faible coût.

---

### Détail de chaque modèle

#### Opus 4 — Le plus puissant

- Conçu pour les **tâches très complexes** : analyse multi-étapes, raisonnement scientifique, planification stratégique.
- Supporte le **mode étendu de pensée** (*extended thinking*) : le modèle peut "réfléchir" plusieurs secondes avant de répondre pour des problèmes difficiles.
- Idéal quand la **qualité prime sur la vitesse ou le coût**.

**Cas d'usage typiques :**

- Audit de sécurité d'une base de code entière
- Refactoring architectural complexe
- Analyse de documents juridiques ou scientifiques
- Génération de plans d'implémentation détaillés

```python
import anthropic

client = anthropic.Anthropic()

# Exemple avec Opus pour un raisonnement complexe
message = client.messages.create(
    model="claude-opus-4-7",
    max_tokens=4096,
    messages=[
        {
            "role": "user",
            "content": "Analyse les failles potentielles de sécurité dans cette architecture microservices et propose un plan de correction."
        }
    ]
)

print(message.content[0].text)
```

---

#### Sonnet 4 — L'équilibre parfait

- Le **modèle recommandé par défaut** pour la majorité des applications.
- Offre une excellente **qualité de réponse** avec une vitesse acceptable et un coût raisonnable.
- Très bon pour le **code**, la **rédaction** et les tâches générales.
- C'est le modèle utilisé par **Claude Code** par défaut.

**Cas d'usage typiques :**

- Aide au développement quotidien (debug, code review, génération de fonctions)
- Rédaction et résumé de documents
- Chatbot dans une application
- Assistant dans un IDE

```python
import anthropic

client = anthropic.Anthropic()

# Exemple avec Sonnet pour du code — usage quotidien recommandé
message = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=2048,
    system="Tu es un assistant développeur expert en Python.",
    messages=[
        {
            "role": "user",
            "content": "Écris une fonction Python qui valide un numéro de carte de crédit avec l'algorithme de Luhn."
        }
    ]
)

print(message.content[0].text)
```

---

#### Haiku 4 — Le plus rapide

- Conçu pour la **vitesse et le faible coût** : idéal pour les systèmes qui traitent beaucoup de requêtes.
- Parfait pour des **tâches simples et répétitives** où la qualité maximale n'est pas requise.
- Utilisé dans les pipelines automatisés, les classifications, les réponses courtes.

**Cas d'usage typiques :**

- Modération automatique de contenu
- Classification de texte à grande échelle
- Extraction d'entités simples dans des documents
- Chatbot de premier niveau (FAQ, réponses prédéfinies)
- Résumés rapides de courts textes

```python
import anthropic

client = anthropic.Anthropic()

# Exemple avec Haiku pour une tâche simple et rapide
message = client.messages.create(
    model="claude-haiku-4-5-20251001",
    max_tokens=256,
    messages=[
        {
            "role": "user",
            "content": "Classe ce message comme POSITIF, NÉGATIF ou NEUTRE : 'Le produit est correct mais la livraison était en retard.'"
        }
    ]
)

print(message.content[0].text)
# Résultat attendu : NÉGATIF (ou NEUTRE selon le contexte)
```

---

### Illustration : choisir le bon modèle

```
Quelle est la complexité de la tâche ?
            │
            ▼
    ┌───────────────┐
    │  Très élevée  │ ──► Opus 4  (claude-opus-4-7)
    │  (raisonnement│      Analyse complexe, audit, architecture
    │   profond)    │
    └───────────────┘
            │
    ┌───────────────┐
    │   Moyenne     │ ──► Sonnet 4  (claude-sonnet-4-6)
    │  (usage       │      Code, rédaction, usage général
    │   général)    │
    └───────────────┘
            │
    ┌───────────────┐
    │   Faible /    │ ──► Haiku 4  (claude-haiku-4-5-20251001)
    │   Répétitive  │      Classification, FAQ, pipelines
    └───────────────┘
```

---

### Comparatif rapide

| Critère | Opus 4 | Sonnet 4 | Haiku 4 |
|---------|--------|----------|---------|
| **Qualité du raisonnement** | ★★★★★ | ★★★★☆ | ★★★☆☆ |
| **Vitesse de réponse** | ★★☆☆☆ | ★★★★☆ | ★★★★★ |
| **Coût par token** | Élevé | Moyen | Faible |
| **Context window** | 200 000 tokens | 200 000 tokens | 200 000 tokens |
| **Extended thinking** | Oui | Non | Non |
| **Idéal pour** | Analyse profonde | Développement | Automatisation |

> **Context window de 200 000 tokens** ≈ environ 150 000 mots, soit un roman entier. Tous les modèles Claude 4 partagent cette capacité.

---

## Comment Claude fonctionne : le principe de prompt

Tout échange avec Claude se fait via un **prompt** — un message texte envoyé au modèle. Claude lit le contexte complet de la conversation (appelé **context window**) et génère une réponse.

```
Utilisateur → [Prompt] → Claude (LLM) → [Réponse]
```

### Rôles dans une conversation

| Rôle | Description |
|------|-------------|
| `system` | Instructions de base données à Claude au départ (son comportement, ses limites) |
| `user` | Message de l'utilisateur |
| `assistant` | Réponse de Claude |

### Exemple minimal via l'API Anthropic (Python)

```python
import anthropic

client = anthropic.Anthropic()  # utilise la clé ANTHROPIC_API_KEY

message = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=1024,
    messages=[
        {"role": "user", "content": "Explique-moi ce qu'est un LLM en 2 phrases."}
    ]
)

print(message.content[0].text)
```

**Résultat attendu :** Claude répond en langage naturel avec une explication concise d'un LLM.

---

## Qu'est-ce que Claude Code ?

**Claude Code** est l'interface en ligne de commande (CLI) officielle d'Anthropic qui intègre Claude directement dans le terminal du développeur. C'est un **agent de développement** : il peut lire, écrire et modifier des fichiers, exécuter des commandes shell, faire des recherches dans le code, et interagir avec Git.

```
Développeur → [Terminal / IDE] → Claude Code CLI → Claude (LLM) → Actions sur les fichiers
```

### Installation

```bash
npm install -g @anthropic-ai/claude-code
```

```bash
claude  # démarre une session interactive
```

### Capacités de Claude Code

| Capacité | Description |
|----------|-------------|
| Lecture de fichiers | Lit et analyse le code source du projet |
| Écriture / édition | Modifie des fichiers directement |
| Bash | Exécute des commandes shell |
| Git | Gère les commits, diffs, branches |
| Recherche | Cherche des symboles, patterns dans le code |
| Agents | Lance des sous-agents pour des tâches parallèles |
| MCP | Se connecte à des outils externes (Gmail, Drive, etc.) |

### Exemple de session Claude Code

```bash
# Dans le terminal, à la racine d'un projet
claude

# Prompt utilisateur dans la session interactive :
> Trouve tous les fichiers TypeScript qui utilisent useState et liste les composants concernés.
```

Claude Code va alors :
1. Scanner les fichiers `.tsx` / `.ts` du projet
2. Identifier les imports de `useState`
3. Retourner la liste des composants avec les numéros de ligne

---

## Illustration : architecture générale

```
┌─────────────────────────────────────────────────┐
│                 Développeur                      │
└────────────────────┬────────────────────────────┘
                     │ prompt / question
                     ▼
┌─────────────────────────────────────────────────┐
│            Claude Code (CLI / IDE)              │
│  - Lit les fichiers locaux                      │
│  - Exécute des commandes shell                  │
│  - Gère Git                                     │
└────────────────────┬────────────────────────────┘
                     │ envoie le contexte
                     ▼
┌─────────────────────────────────────────────────┐
│              Claude (LLM - Anthropic)           │
│  - Raisonne sur le contexte                     │
│  - Génère du texte / du code                    │
│  - Décide des actions à effectuer               │
└─────────────────────────────────────────────────┘
```

---

## Différences entre Claude (chat) et Claude Code (CLI)

| | Claude (chat web) | Claude Code (CLI) |
|---|---|---|
| **Interface** | Navigateur (claude.ai) | Terminal / IDE |
| **Accès aux fichiers** | Non (sauf upload manuel) | Oui, directement |
| **Exécution de code** | Non | Oui (bash) |
| **Intégration Git** | Non | Oui |
| **Usage principal** | Conversation, rédaction | Développement logiciel |

---

## Points clés à retenir

- Claude est un LLM d'Anthropic disponible en plusieurs versions (Opus, Sonnet, Haiku).
- Il fonctionne par **prompts** — tout le contexte de la conversation est envoyé à chaque requête.
- **Claude Code** est la CLI qui transforme Claude en assistant de développement agentique.
- Claude Code peut lire, écrire des fichiers, exécuter des commandes et gérer Git de façon autonome.
- Le modèle recommandé par défaut est **Sonnet** (`claude-sonnet-4-6`).

---

## 📚 Sources

- [Claude Code — Documentation officielle Anthropic](https://docs.anthropic.com/fr/docs/claude-code/overview)
- [Modèles Claude disponibles — Anthropic](https://docs.anthropic.com/fr/docs/about-claude/models/overview)
- [API Anthropic — Premiers pas](https://docs.anthropic.com/fr/docs/initial-setup)
- [Claude Code sur GitHub (npm)](https://www.npmjs.com/package/@anthropic-ai/claude-code)
