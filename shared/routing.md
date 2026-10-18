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

✅ **AUDIT 11/10/2026 : le stack réel = `run.py watch` (Hermes Weather Bot) + `observer.py`, tous deux UP.** (run.py watch PID 545358 depuis 08/10, observer.py PID 69478 depuis 05/10, supervisés par `weather-watchdog.sh` — dernier run OK 11/10 10:33).

Les 3 bots legacy (whale grinding, grinding, weather ladder) sont **obsolètes** : derniers logs août 2026, `bot_v3.py` archivé (artefact mort du 31/08 remplacé par `run.py watch`), `real_trader_v2.py` présent à `~/real_trader_v2.py` mais plus lancé. Le cron `auto-restart-bots` n'existe toujours pas (devenu inutile : le watchdog couvre les 2 bots vivants).

| Bot | Type | Restart | Statut |
|-----|------|---------|--------|
| Hermes Weather Bot | `run.py watch` (dans `~/hermes_weatherbot/`) | `weather-watchdog.sh` (cron 5min) | ✅ UP (PID 545358, depuis 08/10) |
| Observer ask_ladder | `observer.py` (dans `~/hermes_weatherbot/`) | `weather-watchdog.sh` | ✅ UP (PID 69478, depuis 05/10) |
| Whale Grinding (legacy) | `real_trader_v2.py --strategy whale_grinding` | — | ⚰️ Obsolète, down depuis ~20/08/2026 |
| Grinding weather (legacy) | `real_trader_v2.py --strategy grinding` | — | ⚰️ Obsolète, down depuis ~20/08/2026 |
| Weather Ladder (legacy) | `bot_v3.py` | — | ⚰️ Obsolète (archivé, remplacé par run.py watch) |

**Problèmes ouverts :**
1. Duplicata weather-watchdog : `scripts/watchdog.sh` (double `scripts/` → 1561 échecs cumulés) → à supprimer ; `weather-watchdog.sh` (OK)
2. Le pré-script d'auto-sync grepait les noms legacy (`real_trader_v2`, `bot_v3.py`) → faux `0/3`/`NO_BOTS_RUNNING` dans les snapshots ; pattern corrigé le 11/10 dans `auto-sync-dominical.sh` (`run.py watch`, `observer.py`, `scan_wallets_v11`)

## Messages automatiques entrants

- **Emails Christine/Rémy** → check-email.py (cron no_agent) → réponse automatique
- **Cron reports** → delivery local sauf mention contraire
- **Brain sync** → brain-sync-pull toutes les 15min

## Profiles & Modèle

- Tous les agents : `deepseek/deepseek-v4-flash` (OpenRouter)
- Profiles isolés : sessions, mémoire, cron, skills indépendants
- Skills partagées depuis `~/.hermes/skills/`