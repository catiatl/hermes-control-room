# hermes-trading — Inventory

## Role
Agent Trading Polymarket : copy trading réel (whale grinding + grinding), weather bot, discovery de wallets, analyse de stratégies.

## Where It Runs
- Host: ThinkCenter (localhost)
- Profile: `trading` (`~/.hermes/profiles/trading/`)
- Modèle: `deepseek/deepseek-v4-flash` (OpenRouter)
- Wallet réel: `0x4a6e49071ba0dd9fcff5e0efe65626bbd99420fa` ($4.80 USDC) — **vérifié 11/10/2026 via data-api : 0 positions OPEN et 0 CLOSED** (wallet inactif). Solde USDC non re-vérifiable : endpoints balance (gamma-api/data-api) en 404.

## Architecture — Bots 24/7

✅ **AUDIT 11/10/2026 : stack actif = `run.py watch` + `observer.py`, tous deux UP** (depuis 08/10 et 05/10). Les 3 bots legacy sont obsolètes — dernière activité août 2026, `bot_v3.py` archivé (artefact mort remplacé par `run.py watch`), `real_trader_v2.py` présent mais plus lancé. `auto-restart-bots` n'existe pas (le watchdog couvre les 2 bots vivants).

| Bot | Code | Capital | Stratégie | Statut |
|-----|------|---------|-----------|--------|
| **Hermes Weather Bot** | `run.py watch` | $62.04 paper (journal 09/10) | Live weather bot GGhff | ✅ UP (PID 545358) |
| **Observer ask_ladder** | `observer.py` | — | Liquidité ask ladder | ✅ UP (PID 69478) |
| **Whale Grinding (legacy)** | `real_trader_v2.py --strategy whale_grinding` | $5.47 (last log 20/08) | Copy entry+exit wallets politiques (9 wallets) | ⚰️ Obsolète |
| **Grinding (legacy)** | `real_trader_v2.py --strategy grinding` | $6.02 (last log 20/08) | Copy entry+exit 1 wallet weather | ⚰️ Obsolète |
| **Weather Ladder (legacy)** | `bot_v3.py` | Paper | Scan 25 villes, multi-bucket DCA | ⚰️ Archivé (remplacé run.py watch) |
| **Scan wallets V10/V11** | `scan_wallets_v11.py` | — | Découverte hebdo | Cron OK |

## Wallet Verification Pipeline

Tous les wallets du panel sont vérifiés via API positions fermées (pas leaderboard) :
1. **PnL réel** — `data-api.polymarket.com/positions?user={addr}&status=CLOSED`
2. **Holding time** — ≥5min pour être copiable
3. **Niche** — politics, weather, sports

## Whale Grinding Panel (09 wallets politiques vérifiés)

| Wallet | PnL réel | WR | Priorité |
|--------|:--------:|:--:|:--------:|
| anon_63d43b 🏆 | +$3,997K | 89% | A (politics,sports) |
| anon_6e2c0e | +$325K | 59% | A |
| anon_e4cfbb | +$167K | 98% | A (inactif) |
| anon_9be90e | +$67K | 98% | A (inactif) |
| anon_5a218c | +$769K | 43% | B |
| anon_06dc51 | +$226K | 37% | B |
| anon_43cb4a | +$170K | 32% | B |
| anon_9d57c4 | +$41K | 62% | B |
| anon_e9c631 | +$16K | 53% | B |

## Grinding Panel (1 wallet weather)

| Wallet | PnL | WR | Hold | Notes |
|--------|:---:|:--:|:----:|-------|
| anon_f27de6 | +$1.7K | 52% | ~8h | Weather uniquement |

## Faux positifs documentés (ne PAS ajouter)

| Wallet | Leaderboard | PnL réel | Raison |
|--------|:----------:|:--------:|--------|
| Domer | +$403K | **-$889K** | Unrealized PnL dans leaderboard |
| Diamond Whale | +$8.76M | **-$104K** | Idem |
| Bland-Dynamite | +$17.7K | +$17.7K | Scalpeur 10s, pas copiable |

## Cron Jobs (sur default profile)

Les bots 24/7 sont gérés par le cron `auto-restart-bots` (toutes les 5min). Le cron scan wallets tourne le dimanche.

## Skills
- `trading/polymarket-weather-trading` — Weather bot, ladder, merge
- `trading/polymarket-real-execution` — Wallet réel, orders CLOB
- `trading/polymarket-btc-directional` — BTC 5min strategies
- `trading/polymarket-consensus-whales` — Analyse whales
- `trading/polymarket-osiris-trigger` — News trigger
- `polymarket-trading/*` — Copy trading, weather grinder, whale rider
- `research/polymarket` — Polymarket API
- `trading/trading-journal` — Journal de bord

## Memory clé
- Toujours vérifier PnL via API positions fermées AVANT d'ajouter un wallet
- Pas de scalpeurs (in/out <60s = pas copiable)
- Safety > speed — valider avant déploiement
- Flow veut des RÉSULTATS, pas des itérations sans fin