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

## Bots 24/7

⚠️ **AUDIT 06/09/2026 : 0/3 bots en vie.** Le cron `auto-restart-bots` a disparu de la cron list. Sans mécanisme de restart, les bots Polymarket sont tous tombés (dernière activité août 2026). Le seul bot qui tourne est Hermes Weather Bot (`run.py watch`, géré par `weather-watchdog`).

| Bot | Type | Restart | Statut |
|-----|------|---------|--------|
| Whale Grinding | `real_trader_v2.py --strategy whale_grinding` | **MANQUANT** | 🔴 Down depuis ~20/08/2026 |
| Grinding weather | `real_trader_v2.py --strategy grinding` | **MANQUANT** | 🔴 Down depuis ~20/08/2026 |
| Weather Ladder | `bot_v3.py` | Cron 4x/jour | 🔴 Down (path error: file à `~/repos/` pas `~/hermes_weatherbot/`) |

**Problèmes ouverts :**
1. Le cron `auto-restart-bots` n'existe plus dans la cron list — nécessite recréation
2. `bot_v3.py` est dans `~/repos/hermes_weatherbot/` mais le cron pointe vers `~/hermes_weatherbot/` — path incorrect
3. Duplicata de weather-watchdog : un avec path `scripts/watchdog.sh` (double `scripts/` → erreur), un avec `weather-watchdog.sh` (OK)

## Messages automatiques entrants

- **Emails Christine/Rémy** → check-email.py (cron no_agent) → réponse automatique
- **Cron reports** → delivery local sauf mention contraire
- **Brain sync** → brain-sync-pull toutes les 15min

## Profiles & Modèle

- Tous les agents : `deepseek/deepseek-v4-flash` (OpenRouter)
- Profiles isolés : sessions, mémoire, cron, skills indépendants
- Skills partagées depuis `~/.hermes/skills/`