# hermes-trading - Inventory

## Role
Agent Trading : veille marchés, paper trading stocks US/HK, Polymarket, Solana, risk management.

## Where It Runs
- Host: ThinkCenter (localhost)
- Deployment style: Hermes profile (`--profile trading`)
- Profile dir: `~/.hermes/profiles/trading/`
- Data dir: `~/trading/`

## Skills
| Skill | Usage |
|---|---|
| `trading/trading-system` | Système de trading complet |
| `trading/trading-risk-management` | Stop-loss, trailing, TP |
| `trading/veille-trading-automatique` | Veille quotidienne combinée |
| `research/polymarket` | Polymarket data + paper trading |
| `research/last30days` | Recherche tendances |

## Memory
- Positions paper trading (AAPL, SPY, QQQ, SOL)
- Stratégies ORB, momentum
- Wallets : Coinbase, MetaMask Polygon, IBKR, Hyperliquid
- Logs PnL, analytics

## Cron Jobs
| Cron | Frequence | Type |
|---|---|---|
| paper-trading-aapl | 22:20 lun-ven | no_agent |
| paper-trading-orb-spy | 22:10 lun-ven | no_agent |
| paper-trading-orb-qqq | 22:15 lun-ven | no_agent |
| paper-trading-orb-ezu | 22:12 lun-ven | no_agent |
| paper-trading-solana | 22:00 lun-ven | no_agent |
| paper-trading-ford | 22:25 lun-ven | no_agent |
| signal-hk-orb | 10:30 lun-ven | no_agent |
| recap-midi | 10:35 lun-ven | no_agent |
| recap-soir | 22:35 lun-ven | no_agent |
| analyse-quotidienne | 22:30 lun-ven | no_agent |
| polymarket-paper-bot | */30 2h | no_agent |
| polymarket-recap | 22h quotidien | no_agent |
| review-audit-24h | 22:30 lun-ven | LLM |
| position-monitor | */15 3-21 lun-ven | no_agent |

## Messaging
- **Flow** — via Telegram

## Allowed Work
- Analyse actions US/HK
- Paper trading (AAPL, SPY, QQQ, EZU, SOL, Ford)
- Polymarket paper trading
- Veille trading, signaux
- Risk management

## Forbidden Work
- Décisions d'achat/vente réelles sans validation
- Accès comptes bancaires/vrais wallets
- TL, content, devops

## Owner
- Flow
