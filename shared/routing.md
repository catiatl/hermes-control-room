# Règles de Routage — Catia Orchestrator

## Qui parle à qui ?

| Personne | Agent cible | Canal |
|---|---|---|
| **Christine** | hermes-tl | Email → check-email → Telegram |
| **Rémy** | hermes-tl | Email → check-email → Telegram |
| **Flow (sujet TL)** | hermes-tl | Telegram direct |
| **Flow (sujet trading)** | hermes-trading | Telegram direct |
| **Flow (sujet TikTok/content)** | hermes-content | Telegram direct |
| **Flow (sujet dev/projets)** | hermes-dev | Telegram direct |
| **Flow (sujet mélangé/général)** | Catia (orchestrateur) | Telegram direct |

## Comment Catia route

Quand Flow parle à Catia :

| Il dit... | Je route vers... |
|---|---|
| "devis", "livraison", "client", "Christine", "email", "recrutement", "gazole" | hermes-tl |
| "AAPL", "SPY", "SOL", "trading", "paper", "polymarket", "position" | hermes-trading |
| "TikTok", "vidéo", "invention", "logo", "montage", "inventory lab" | hermes-content |
| "audit", "màj", "sécurité", "CubageLogic", "Bénin", "dropshipping" | hermes-dev |
| Question générale, conversation, organisation | Catia répond direct |

## Content peut servir TL

hermes-content peut créer des vidéos commerciales/promos pour TL.
C'est un **service** que content rend à TL — pas une dépendance technique.

## Priorités Flow

1. TL
2. Projets money (CubageLogic, dropshipping, Bénin)
3. Outils et infra
4. Trading
