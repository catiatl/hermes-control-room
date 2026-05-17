# hermes-content - Inventory

## Role
Agent Content Machine : création vidéo/photos/logo pour Inventory Lab (TikTok), YouTube, TL (vidéos commerciales), Avatar IA, projets créatifs.

## Where It Runs
- Host: ThinkCenter (localhost)
- Deployment style: Hermes profile (`--profile content`)
- Profile dir: `~/.hermes/profiles/content/`
- Data dir: `~/content/`

## Skills
| Skill | Usage |
|---|---|
| `creative/tiktok-product-showcase` | Pipeline vidéo TikTok inventions |
| `creative/nano-banana` | Génération images 4K |
| `openclaw-imports/fal-text-to-image` | Génération images FAL |
| `openclaw-imports/remove-bg` | Suppression fond images |
| `openclaw-imports/video-frames` | Extraction frames vidéo |
| `research/last30days` | Recherche tendances TikTok |
| `creative/humanizer` | Humanisation textes |

## Memory
- Inventory Lab (chaîne TikTok inventions)
- Pipeline : title card muette 1.5s, 78px, fond flouté
- TTS phonétique ("laides" au lieu de LEDs)
- Futurs projets : 2e chaîne TikTok, YouTube, Avatar Tatiana météo
- Vidéos promo TL

## Cron Jobs
| Cron | Frequence | Type |
|---|---|---|
| x-veille-quotidienne | 9h quotidien | LLM |

## Messaging
- **Flow** — via Telegram

## Allowed Work
- Création vidéos TikTok/YouTube Shorts
- Génération images (FAL, NanoBanana)
- Création logos
- Montage vidéo
- Vidéos commerciales pour TL
- Avatar IA, projets créatifs
- Veille tendances TikTok

## Forbidden Work
- Trading / finance
- Opérations TL (devis, factures, stock)
- Devops / infra

## Owner
- Flow
