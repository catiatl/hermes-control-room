# Flow's Agent Control Room

**Operator : Flow**
**Orchestrateur : Catia (default profile Hermes)**

## Architecture

```
Flow ──► Telegram ──► Catia (orchestrateur uniquement)
                          │
                    ┌─────┼──────┬───────────┐
                    ▼     ▼      ▼           ▼
                  tl   trading  content    dev
```

Flow parle UNIQUEMENT à **Catia** sur Telegram. Catia analyse le sujet et spawn le sous-agent approprié.

## Les 4 Sous-Agents

| Agent | Rôle | Qui parle | Priorité |
|-------|------|-----------|----------|
| **[tl](agents/tl/inventory.md)** | Transport Logic (métier, finance, RH, email) | Christine, Rémy, Flow | ⭐ 1 |
| **[trading](agents/trading/inventory.md)** | Stocks US/HK, Polymarket, Solana, paper | Flow | ⭐ 2 |
| **[content](agents/content/inventory.md)** | TikTok Inventory Lab, vidéos, images, logo | Flow | ⭐ 3 |
| **[dev](agents/dev/inventory.md)** | SaaS, infra, sécurité, projets (Bénin, dropshipping) | Flow | ⭐ 4 |

## Règles de Routage

Voir `shared/routing.md` pour les règles complètes.

## Catia — Orchestrateur

Catia est l'interface unique de Flow. Toute communication passe par elle.

**Compétences :**
- Analyse du sujet du message
- Délégation au bon sous-agent
- Synthèse des résultats
- Réponse directe pour les questions simples

**Ne fait pas :**
- N'a pas ses propres skills métier (ceux des sous-agents)
- Ne passe pas par-dessus les sous-agents pour les tâches spécialisées
- Ne prend pas de décisions financières/strategiques sans Flow

## Projets Non Alloués (dans dev en attendant)

- CubageLogic
- Dropshipping
- Bénin (auberge, localogic, achatlogic, ONG)
- CEO & Marketing

## Liens Rapides

- `hermes --profile tl chat`   — Parler à TL
- `hermes --profile trading chat`   — Parler à Trading
- `hermes --profile content chat`   — Parler à Content
- `hermes --profile dev chat`   — Parler à Dev/Projets
