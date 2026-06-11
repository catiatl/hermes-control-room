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
ps aux | grep hermes_cli | grep -v grep
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

## Crons

Tous les crons sont dans le **default profile**. Voir :
```bash
hermes cron list
```

## Profiles secondaires (pour terminal uniquement)

```bash
hermes --profile tl chat -q "question"
hermes --profile trading chat -q "question"
hermes --profile content chat -q "question"
hermes --profile dev chat -q "question"
```

Pas de gateways dédiés. Pas de crons dans ces profiles.

## Architecture

```
Toi → Telegram → Catia (gateway default) ← systemd auto-restart
                    │
                    ├── TL direct (mémoire TL dans ~/.hermes/memories/)
                    ├── Profiles pour terminal uniquement
                    └── 28 crons dans default
```

## Test rapide après redémarrage

```bash
# 1. Gateway en vie ?
systemctl --user is-active hermes-gateway.service
# 2. Logs OK ?
tail -5 ~/.hermes/logs/gateway.log
# 3. Crons schedulés ?
hermes cron list | grep "state: scheduled" | wc -l
# 4. Toi sur Telegram ?
# → envoie un message de test
```
