# 🏗️ Flow's Agent Control Room

**Operator : Flow** — Documentation et gouvernance des agents Hermes.

## Architecture

```
Flow ────► Catia (orchestrateur, optionnel)
                │
      ┌─────────┼─────────┬───────────┐
      ▼         ▼         ▼           ▼
   hermes-tl  hermes-t  hermes-cm  hermes-dev
```

## Les 4 Agents

| Agent | Rôle | Qui parle | Priorité |
|---|---|---|---|
| **[hermes-tl](agents/tl/inventory.md)** | Transport Logic (métier, finance, RH, email) | Christine, Rémy, Flow | ⭐ 1 |
| **[hermes-trading](agents/trading/inventory.md)** | Stocks US/HK, Polymarket, Solana, paper | Flow | ⭐ 2 |
| **[hermes-content](agents/content/inventory.md)** | TikTok Inventory Lab, vidéos, images, logo | Flow | ⭐ 3 |
| **[hermes-dev](agents/dev/inventory.md)** | SaaS, infra, sécurité, projets (Bénin, dropshipping) | Flow | ⭐ 4 |

## Règles de Routage

Voir `shared/routing.md` pour les règles complètes.

## Projets Non Alloués (dans hermes-dev en attendant)
- CubageLogic
- Dropshipping
- Bénin (auberge, localogic, achatlogic, ONG)
- CEO & Marketing

## Liens Rapides

- `hermes --profile tl chat` — Parler à TL
- `hermes --profile trading chat` — Parler à Trading
- `hermes --profile content chat` — Parler à Content
- `hermes --profile dev chat` — Parler à Dev/Projets
