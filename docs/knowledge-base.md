# 🧠 Knowledge Base — Catia Orchestrator
> Fichier persistant. Mis à jour le 19 mai 2026.

## Architecture
- **Gateway**: hermes-gateway.service (systemd user)
- **Profiles**: default (Catia), tl, trading, content, dev
- **Scripts**: ~/.hermes/scripts/ (43 fichiers, backup → ~/hermes-backup/scripts/)
- **Config**: ~/.hermes/config.yaml

## Routage
| Personne | Agent | Comment |
|---|---|---|
| Christine | hermes-tl | Email → Telegram |
| Rémy | hermes-tl | Email → Telegram |
| Flow (TL) | hermes-tl | Devis, livraisons, RH, email |
| Flow (trading) | hermes-trading | US/HK/Polymarket/SOL |
| Flow (content) | hermes-content | TikTok, vidéos, Inventory Lab |
| Flow (dev) | hermes-dev | CubageLogic, Bénin, dropshipping, infra |

## Crons Trading (22h-22h45 CET, Lun-Ven)
- 22:00 — TOTO, SOL, analyse-veille
- 22:05 — MACD AAPL
- 22:07 — Gap Fill QQQ
- 22:10 — SPY ORB
- 22:12 — IWM ORB (appelle EZU en réalité)
- 22:15 — QQQ ORB
- 22:20 — AAPL momentum
- 22:25 — Ford WSB
- 22:30 — daily audit + analyse quotidienne
- 22:45 — VWAP SPY

## Autres Crons
- Position monitor: */15 3-21 * * 1-5
- Polymarket bot: toutes les heures
- Check participants: lundi 10h
- Backup hebdo: lundi 6h
- Prix gazole: 7h lun-ven
- Veille semi-conducteurs: lundi 18h
- Recrutement: samedi/dimanche 10h+16h
- Veille-agenda TL: 18h quotidien

## Infrastructure Partagée
- **utils.py**: compute_rsi, atomic_save, is_market_day, compute_sharpe, compute_max_drawdown, compute_position_size, check_circuit_breaker, signal_circuit_breaker, simulate_fill
- **orb_engine.py**: run_orb(df, config) → dict
- **strategy_library.py**: 10 strategies (turtle, golden_cross, rsi_mr, bollinger, dual_momo, inside_day, gap_fill, macd, supertrend, rsi_div)
- **backtest_engine.py**: test toutes les strategies × tickers, ranking
- **paper_trading_strategy.py**: script unique paramétrique (ticker + strategy_name)
- Circuit breaker: ~/.hermes/.circuit_breaker (perte > -3% → tout stop)

## MiroFish
- Multi-agent LLM simulation, on-demand
- 5-6 agents max, ~$0.003/run
- Ne pas utiliser > 1-2×/semaine
- Utiliser UNIQUEMENT après veille, pas à la place

## À SAVOIR
- Telegram limité à 20MB pour fichiers
- Email: fliaigre@transportlogic.fr (SMTP O2Switch)
- Proxy O2Switch: impedito ad blocking! (Bloque scraping)
- Gateway log: journalctl --user -u hermes-gateway.service
- Recovery: ~/control-room/docs/recovery.md
