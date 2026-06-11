# hermes-content — Inventory

## Role
Agent Content Machine : création vidéo TikTok, images, logos, contenu pour Inventory Lab, TL, projets créatifs.

## Where It Runs
- Host: ThinkCenter (localhost)
- Profile: `content` (`~/.hermes/profiles/content/`)
- Modèle: `deepseek/deepseek-v4-flash` (OpenRouter)
- Data dir: `~/content/`

## Skills
| Skill | Usage |
|-------|-------|
| `creative/tiktok-product-showcase` | Pipeline vidéo TikTok split-screen pour showcase d'inventions |
| `creative/faceless-content-machine` | Production automatisée de contenu faceless |
| `creative/nano-banana` | Génération/édition d'images (Gemini 3 Pro) |
| `creative/humanizer` | Humaniser texte, enlever AI-isms |
| `creative/excalidraw` | Diagrammes hand-drawn |
| `creative/architecture-diagram` | SVG dark-themed architecture diagrams |
| `creative/video-editing` | Montage vidéo |
| `creative/video-clip-pipeline` | Pipeline clips vidéo |
| `creative/manim-video` | Animations mathématiques (3Blue1Brown style) |
| `video-use/video-use` | Édition vidéo par conversation |
| `research/last30days` | Recherche tendances TikTok |
| `ai-marketing/short-form-pipeline` | Pipeline short-form content |
| `ai-marketing/content-eval` | Évaluation de contenu |

## TikTok Pipeline
- Sous-titres: 38px, 3-4 mots max, tout en bas (MarginV=60)
- Pack post complet obligatoire : description + hashtags + CTA
- Pipeline custom (pas MoneyPrinter)

## Outils créatifs
- Image generation via FAL / Gemini
- Face-swap via InsightFace (skill face-swap créée)
- CloakBrowser v0.31.3 installé (stealth Chromium)

## Cron Jobs
- (contenu créé à la demande, pas de cron actif)

## Futures explorations
- Winamax poker bot
- LeBonCoin automation