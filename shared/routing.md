# Règles de Routage — Catia Orchestrator

## Architecture

```
Flow ──► Telegram ──► Catia (orchestrateur, default profile)
                          │
                          ├── hermes --profile tl chat -q "..."      → Agent TL
                          ├── hermes --profile trading chat -q "..." → Agent Trading
                          ├── hermes --profile content chat -q "..." → Agent Content
                          └── hermes --profile dev chat -q "..."     → Agent Dev
```

Flow parle UNIQUEMENT à Catia sur Telegram. Catia spawn les sous-agents via CLI uniquement (pas de gateway pour les sous-agents). Bots 24/7 gérés par auto-restart cron.

## Comment Catia route

| Flow dit... | Sous-agent | Domaine |
|-------------|------------|---------|
| "devis", "livraison", "client", "Christine", "Rémy", "email", "recrutement", "gazole", "chiffrage", "facture", "pennylane", "stock", "palette", "transport", "TL" | **tl** | Transport Logic |
| "Polymarket", "whale", "grinding", "copy", "PnL", "position", "wallet", "scan", "weather", "température", "market", "trade", "trading", "bot" | **trading** | Polymarket |
| "TikTok", "vidéo", "invention", "logo", "montage", "inventory lab", "image", "content" | **content** | TikTok / Content |
| "CubageLogic", "dropshipping", "Bénin", "git", "deploy", "api", "hermes", "install", "serveur", "code" | **dev** | Dev & Projets |
| Question simple (météo, date, rappel, conversation) | Catia répond direct | — |
| Brain vault, mémoire, GitHub control room | Catia répond direct | — |

## Christine & Rémy

Ils parlent à **l'agent TL**, pas à Catia. Leur canal : email → check-email.py (cron no_agent) → réponse automatique. Ne passe pas par l'orchestration Catia.

## Flux d'orchestration

1. Flow envoie un message sur Telegram
2. Catia analyse le sujet (mots-clés, contexte)
3. Si tâche spécialisée → spawn `hermes --profile <name> chat -q "tâche"`
4. Catia récupère le résultat et synthétise la réponse à Flow
5. Si question simple → Catia répond direct
6. Si tâche multi-domaine → spawner chaque agent séquentiellement, synthétiser

## Priorités Flow

1. **TL** (métier — priorité #1, Christine/Rémy en direct)
2. **Trading** (Polymarket real + paper, 3 bots actifs)
3. **Projets** (CubageLogic, dropshipping, Bénin)
4. **Infra / Hermes**
5. **Content / TikTok**

## Bots 24/7 (gérés par auto-restart)

Ces bots tournent en continu et ne passent PAS par le spawn d'agent :

| Bot | Type | Restart |
|-----|------|---------|
| Whale Grinding | `real_trader_v2.py --strategy whale_grinding` | auto-restart toutes les 5min |
| Grinding weather | `real_trader_v2.py --strategy grinding` | auto-restart toutes les 5min |
| Weather Ladder | `bot_v3.py` scan 4x/jour | Cron 0:05, 6:05, 12:05, 18:05 |

## Messages automatiques entrants

- **Emails Christine/Rémy** → check-email.py (cron no_agent) → réponse automatique
- **Cron reports** → delivery local sauf mention contraire
- **Brain sync** → brain-sync-pull toutes les 15min

## Profiles & Modèle

- Tous les agents : `deepseek/deepseek-v4-flash` (OpenRouter)
- Profiles isolés : sessions, mémoire, cron, skills indépendants
- Skills partagées depuis `~/.hermes/skills/`