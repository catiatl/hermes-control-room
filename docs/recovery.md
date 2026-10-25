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

## Relance Bots (stack actif 11/10/2026)

✅ Le stack actif = **`run.py watch` + `observer.py`** dans `~/hermes_weatherbot/`, supervisés par le cron `weather-watchdog` (toutes les 5min, script `weather-watchdog.sh` — celui-ci est OK ; le duplicata `scripts/watchdog.sh` est cassé, à supprimer). Normalement rien à faire : le watchdog relance.

| Bot | Pour relancer manuellement |
|-----|---------------------------|
| Hermes Weather Bot | `cd ~/hermes_weatherbot && nohup python3 -u run.py watch &` |
| Observer ask_ladder | `cd ~/hermes_weatherbot && nohup python3 -u observer.py &` |

⚠️ **Bots legacy (whale grinding, grinding, weather ladder) : obsolètes** — ne PAS les relancer. `bot_v3.py` est archivé (remplacé par `run.py watch`). `real_trader_v2.py` existe à `~/real_trader_v2.py` (PAS dans `~/repos/hermes_weatherbot/`) mais n'est plus utilisé.

Vérification :
```bash
ps aux | grep -E "run.py watch|observer.py"
hermes cron list | grep weather-watchdog
```

## Bots à surveiller

| Bot | Process | Vérification |
|-----|---------|-------------|
| Hermes Weather Bot | `run.py watch` | `ps aux \| grep "run.py watch"` |
| Observer ask_ladder | `observer.py` | `ps aux \| grep observer.py` |
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
                    
Bots 24/7 (supervisés par weather-watchdog.sh, cron 5min):
- Hermes Weather Bot (run.py watch)
- Observer ask_ladder (observer.py)
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
ps aux | grep -E "run\.py watch|observer\.py" | grep -v grep
# 5. Toi sur Telegram ?
# → envoie un message de test
```

## Backup Brain Vault

Le vault est pushé automatiquement après chaque modification. Sync pull toutes les 15min :

```bash
cd ~/brain && git pull
# Vault GitHub : catiatl/obsidian-brain
```