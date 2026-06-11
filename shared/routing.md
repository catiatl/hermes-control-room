# Règles de Routage — Catia Orchestrator

## Architecture

```
Flow ──► Telegram ──► Catia (moi, orchestrateur, default profile)
                          │
                          ├── hermes --profile tl chat -q "..."      → Agent TL
                          ├── hermes --profile trading chat -q "..." → Agent Trading
                          ├── hermes --profile content chat -q "..." → Agent Content
                          └── hermes --profile dev chat -q "..."     → Agent Dev
```

Flow parle UNIQUEMENT à Catia sur Telegram. Catia spawn les sous-agents via CLI uniquement (pas de gateway pour les sous-agents).

## Comment Catia route

| Flow dit... | Sous-agent | Domaine |
|-------------|------------|---------|
| "devis", "livraison", "client", "Christine", "Rémy", "email", "recrutement", "gazole", "chiffrage", "facture", "pennylane", "stock", "palette" | **tl** | Transport Logic |
| "AAPL", "SPY", "QQQ", "TSLA", "trading", "paper", "alice", "polymarket", "position", "PnL", "trade", "ORB" | **trading** | Trading US |
| "TikTok", "vidéo", "invention", "logo", "montage", "inventory lab", "image", "content" | **content** | TikTok / Content |
| "audit", "màj", "sécurité", "CubageLogic", "Bénin", "dropshipping", "hermes", "install" | **dev** | Dev & Projets |
| Question simple, météo, conversation, rappel, organisation | Catia répond direct | — |

## Christine & Rémy

Ils parlent à **l'agent TL**, pas à Catia. Leur canal : email → check-email → réponse automatique (pas via orchestration Catia).

## Flux d'orchestration

1. Flow envoie un message sur Telegram
2. Catia analyse le sujet
3. Si tâche spécialisée → spawn `hermes --profile <name> chat -q "tâche"`
4. Catia récupère le résultat et synthétise la réponse à Flow
5. Si question simple → Catia répond direct
6. Si tâche multi-domaine → spawner chaque agent séquentiellement, synthétiser

## Priorités Flow

1. **TL** (métier — priorité #1)
2. **Trading** (intensif, plusieurs trades/jour, rapports quotidiens)
3. **Projets** (CubageLogic, dropshipping, Bénin)
4. **Outils et infra**
5. **Content / TikTok**

## Règles d'orchestration

| Situation | Action |
|-----------|--------|
| Question simple (météo, date, rappel) | Répondre direct, pas de spawn |
| Tâche TL | Spawn tl |
| Tâche trading | Spawn trading |
| Tâche content | Spawn content |
| Tâche dev | Spawn dev |
| Tâche multi-domaine | Spawner chaque agent séquentiellement, synthétiser |
| Urgence / action rapide | Spawn direct sans overhead |

## Messages automatiques entrants

- **Emails Christine/Rémy** → check-email.py → analyse → réponse auto (ne passe pas par Catia)
- **Cron reports trading** → delivery local (pas Telegram sauf pre-market-check)
- **Cron reports veille** → delivery local
- **Rapport soir** → 22h35 via cron

## Profiles & Accès

- Tous les agents partagent le même modèle: `deepseek/deepseek-v4-flash` (OpenRouter)
- Profiles isolés: sessions, mémoire, cron, skills indépendants
- Skills partagées depuis `~/.hermes/skills/`
- **Plus de profile `catia`** — supprimé (c'était un clone mort)
