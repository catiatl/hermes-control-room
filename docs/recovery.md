# Récupération Gateway — Catia

## Service systemd

Le gateway Catia tourne via systemd (user service). **Auto-restart** en cas de crash.

```
Service : hermes-gateway.service
Commande : systemctl --user daemon-reload   # après modif du .service
           systemctl --user start hermes-gateway.service
           systemctl --user stop hermes-gateway.service
           systemctl --user status hermes-gateway.service
           systemctl --user restart hermes-gateway.service
Logs : journalctl --user -u hermes-gateway.service -n 50 --no-pager
       journalctl --user -u hermes-gateway.service -f   # follow (live)
```

## Redémarrage manuel (si systemd ne suffit pas)

```bash
systemctl --user stop hermes-gateway.service
# Vérifier qu'aucun process zombie traîne
ps aux | grep hermes | grep -v grep
# Relancer
systemctl --user start hermes-gateway.service
# Vérifier
systemctl --user status hermes-gateway.service --no-pager
```

## Logs Hermes

```
~/.hermes/logs/agent.log      → Activité générale
~/.hermes/logs/errors.log      → Erreurs
~/.hermes/logs/gateway.log     → Gateway
```

## Auto-restart Bots (critique — MANQUANT)

⚠️ **Le cron `auto-restart-bots` a disparu de la cron list avant le 06/09/2026.** Sans lui, aucun restart automatique n'a lieu. Les 3 bots sont down depuis août.

| Bot | Pour relancer manuellement |
|-----|---------------------------|
| Whale Grinding | `cd ~/repos/hermes_weatherbot && python real_trader_v2.py --strategy whale_grinding &` |
| Grinding weather | `cd ~/repos/hermes_weatherbot && python real_trader_v2.py --strategy grinding &` |
| Weather Ladder | `cd ~/repos/hermes_weatherbot && python bot_v3.py &` |

**Pour recréer le cron :**
```bash
hermes cron add --name auto-restart-bots \
  --schedule "*/5 * * * *" \
  --script auto-restart-bots.sh \
  --deliver local
```

Vérification :
```bash
hermes cron list | grep auto-restart-bots
ps aux | grep real_trader_v2
ps aux | grep bot_v3
```

## Bots à surveiller

| Bot | Process | Vérification |
|-----|---------|-------------|
| Whale Grinding | `real_trader_v2.py --strategy whale_grinding` | `ps aux \| grep whale_grinding` |
| Grinding weather | `real_trader_v2.py --strategy grinding` | `ps aux \| grep grinding` |
| Weather Ladder | `bot_v3.py` | Cron `weather-ladder-scan` |
| Scan wallets | `scan_wallets_v11.py` | Cron `scan-wallets-polymarket` (dimanche) |

## Crons

```bash
hermes cron list           → tous les crons
hermes cron list | grep -c "state: scheduled"  → nombre de crons actifs
hermes cron list | grep -i paused             → crons en pause
```

## Profiles secondaires (pour terminal uniquement)

```bash
hermes --profile tl chat -q "question"
hermes --profile trading chat -q "question"
hermes --profile content chat -q "question"
hermes --profile dev chat -q "question"
```

Pas de gateways dédiés pour les sous-agents.

## Architecture

```
Flow → Telegram → Catia (gateway default) ← systemd auto-restart
                    │
                    ├── TL (profile) — spawn CLI
                    ├── Trading (profile) — spawn CLI
                    ├── Content (profile) — spawn CLI
                    └── Dev (profile) — spawn CLI
                    
Bots 24/7 (auto-restart cron):
- Whale Grinding (politique)
- Grinding (weather)
- Weather Ladder (scan 4x/jour)
```

## Test rapide après redémarrage

```bash
# 1. Gateway en vie ?
systemctl --user is-active hermes-gateway.service
# 2. Logs OK ?
tail -5 ~/.hermes/logs/gateway.log
# 3. Crons schedulés ?
hermes cron list | grep "state: scheduled" | wc -l
# 4. Bots en vie ?
ps aux | grep real_trader_v2 | grep -v grep
# 5. Toi sur Telegram ?
# → envoie un message de test
```

## Backup Brain Vault

Le vault est pushé automatiquement après chaque modification. Sync pull toutes les 15min :

```bash
cd ~/brain && git pull
# Vault GitHub : catiatl/obsidian-brain
```