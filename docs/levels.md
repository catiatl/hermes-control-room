# Architecture Levels — Flow's System

## Level 1: Agent Control Room + One Agent
✅ Acquis

## Level 2: Direct Specialist Agents
✅ Acquis (tl, trading, content, dev existent comme profiles)

## Level 3: Orchestrator + Specialists ← NOUS SOMMES ICI
✅ Acquis

Catia (orchestrateur, default profile) = interface unique Flow sur Telegram.
Sous-agents spécialisés spawnés via terminal.
Bots 24/7 Polymarket gérés par auto-restart cron.

## Level 4: Automated Agent Team
🎯 Prochaine étape

Ajouter :
- Workflows récurrents multi-agents (ex: TL demande vidéo promo → content la génère automatiquement)
- Task bus file-based pour handoff asynchrone entre agents
- Monitoring et alerting orchestré (actuellement: auto-restart réactif, pas proactif)
- Rapport PnL consolidé automatique (actuellement en pause)