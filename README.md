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

Flow parle UNIQUEMENT à **Catia** sur Telegram. Catia analyse le sujet et spawn le sous-agent CLI approprié.

## Les 4 Sous-Agents

| Agent | Rôle | Qui parle | Priorité |
|-------|------|-----------|----------|
| **[tl](agents/tl/inventory.md)** | Transport Logic (métier, finance, RH, email) | Christine, Rémy, Flow | ⭐ 1 |
| **[trading](agents/trading/inventory.md)** | Polymarket (real + paper), weather bot, copy grinding | Flow | ⭐ 2 |
| **[content](agents/content/inventory.md)** | TikTok Inventory Lab, vidéos, images, inventons | Flow | ⭐ 3 |
| **[dev](agents/dev/inventory.md)** | SaaS, infra, sécurité, projets (CubageLogic, dropshipping) | Flow | ⭐ 4 |

## Bots actifs (Polymarket)

| Bot | Type | Capital | Status |
|-----|------|---------|--------|
| Whale Grinding | Copy trading politique (9 wallets) | $4.80 USDC réel | ✅ 24/7 |
| Grinding | Copy trading weather (1 wallet) | $4.80 USDC réel | ✅ 24/7 |
| Weather Ladder | Scan 25 villes (bot_v3.py) | Paper | ✅ 4x/jour |
| Scan wallets V10/V11 | Découverte hebdo | — | ✅ Dimanche 08:00 |

## Infrastructure

| Service | Technologie |
|---------|-------------|
| Gateway | systemd user service (auto-restart) |
| Bots 24/7 | Cron auto-restart toutes les 5min |
| Brain vault | `~/brain/` → GitHub catiatl/obsidian-brain |
| Sync vault | Cron brain-sync-pull toutes les 15min |
| Email | check-email.py (3 crons no_agent) |

## Liens Rapides

- `hermes --profile tl chat -q "..."` → Agent TL
- `hermes --profile trading chat -q "..."` → Agent Trading
- `hermes --profile content chat -q "..."` → Agent Content
- `hermes --profile dev chat -q "..."` → Agent Dev
- Vault: `catiatl/obsidian-brain` (GitHub)
- Control Room: `catiatl/hermes-control-room` (GitHub)