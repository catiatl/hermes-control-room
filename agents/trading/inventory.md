# hermes-trading — Inventory

## Role
Agent Trading : veille marchés, paper trading stocks US (Alice bot 10 stratégies), Polymarket paper trading, risk management.

## Where It Runs
- Host: ThinkCenter (localhost)
- Profile: `trading` (`~/.hermes/profiles/trading/`)
- Modèle: `deepseek/deepseek-v4-flash` (OpenRouter)
- Data dir: `~/trading_journal/`

## Capital
- Alice mock-paper: **$10,000** (paper, pas de vrai PnL — simulateur prix fixe)
- Polymarket paper: **$2,000** (41 positions, equity ~$1,900)

## Trading Systems (3 séparés — JAMAIS mélanger)

| Système | Description | Statut |
|---------|-------------|--------|
| **Alice bot** | `alice_trading_v2.py` — 10 stratégies (Turtle, Inside Day, Gap Fill, RSI Mean Rev, MACD, Bollinger, Golden Cross, Dual Momentum, Supertrend, ORB) — bypass UTA one-shot endpoint | ✅ Actif, */30 9h-21h |
| **Polymarket bot** | `polymarket_bot.py` — événements binaires, capital $2k, $50/trade | ✅ Actif, toutes les heures |
| **Bot maison** | Ancien système — supprimé (27 crons effacés) | ❌ Détruit |

## Alice Infrastructure
- MCP SSE: port 3001
- UTA (Unified Trading API): port 47333 (mock-paper, shorts supportés)
- Web UI: port 3002 → proxy 8080 (http://192.168.1.16:8080/dashboard)
- One-shot bypass: `POST /api/trading/uta/mock-paper/wallet/place-order`

## Cron Jobs (31 actifs, profil trading, delivery local sauf pre-market→Telegram)

| Cron | Schedule | Description |
|------|----------|-------------|
| alice-v2-auto | */30 9-21 * * 1-5 | Alice auto trading (10 stratégies) |
| alice-v2-orb | 35 15 * * 1-5 | ORB strategy (ouverture US) |
| alice-v2-close | 30 21 * * 1-5 | Fermeture positions |
| alice-v2-status | 35 22 * * 1-5 | Rapport statut quotidien |
| alice-pre-market-check | 55 8 * * 1 | Pre-market check → Telegram |
| polymarket-paper-bot | 0 * * * * | Polymarket paper trading |
| polymarket-recap | 0 22 * * 0,2,4,6 | Polymarket recap → Telegram |
| veille-communaute | 0 19 * * 1,4 | Reddit veille (17 subs) |
| veille-ia-ecosystem | 0 10 * * 0 | Veille écosystème IA |
| high-value-screener | 0 10 * * 0 | Screener 42 stocks |
| dimanche-brief | 0 10 * * 0 | Brief dominical complet |
| daily-journal | 0 22 * * * | Journal quotidien |
| analyse-stock-quotidienne | 30 22 * * * | Analyse technique complète |
| fly-benin-scan | 0 20 * * 4 | Scan vols Bénin |
| optimization-check | 0 10 * * 0 | Audit config Hermes |
| veille-semi | 0 18 * * 1 | Veille semi-conducteurs |
| + divers scripts support | variés | Journal logging, veille agenda, etc. |

## Skills
- `trading/trading-journal` — Journal de bord quotidien
- `trading/veille-trading-automatique` — Veille combinée Reddit/Polymarket/news
- `trading/mirofish-llm` — Simulation LLM multi-agent pour trading
- `research/polymarket` — Polymarket data
- `research/last30days` — Recherche tendances
- `data-science/jupyter-live-kernel` — Analyse données

## Memory clé
- 3 systèmes séparés : maison (supprimé) + Alice + Polymarket
- Alice et bot maison mêmes paramètres (capital $1k, 5% risk, max 5 positions)
- Polymarket = capital $2k, $50/trade, proba 75%

## Futures évaluations
- TradingAgents (TauricResearch, 82.9k⭐) — framework multi-agent LLM
- QuantDinger — backtesting framework
- Condition: attendre PnL vert avant upgrade
