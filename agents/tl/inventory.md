# hermes-tl - Inventory

## Role
Agent Transport Logic. Gère le métier du transport : devis, factures, livraisons, stock, RH, prospection, email, veille sectorielle.

## Where It Runs
- Host: ThinkCenter (localhost)
- Deployment style: Hermes profile (`--profile tl`)
- Profile dir: `~/.hermes/profiles/tl/`
- Data dir: `~/tl/`

## Skills
| Skill | Usage |
|---|---|
| `openclaw-imports/logic-apis` | StockLogic, DriveLogic APIs |
| `openclaw-imports/tl-chiffrage` | Calcul prix transport |
| `openclaw-imports/tl-devis` | Création devis Pennylane |
| `openclaw-imports/livraison-cmr` | Gestion livraisons + CMR |
| `openclaw-imports/pennylane` | Devis, factures, API comptable |
| `openclaw-imports/vie` | Vérification TVA intracom |
| `openclaw-imports/prospector-lite` | Prospection B2B |
| `openclaw-imports/remove-bg` | Images produits |
| `transport-logic-automation` | Crons métier TL |
| `veille-transport-logic` | Veille carburant, concurrence, lois |

## Memory
- Clients TL, devis en cours, livraisons
- Stock et inventaire
- Agenda interventions
- Historique recrutement

## Cron Jobs
| Cron | Frequence | Type |
|---|---|---|
| veille-agenda | 18h quotidien | no_agent |
| check-participants | 10h lundi | no_agent |
| check-email (jour) | */5 7-21 * * * | no_agent (PAUSED) |
| check-email (nuit) | */30 22-6 * * * | no_agent (PAUSED) |
| recrutement-samedi | 10h,16h samedi | LLM |
| recrutement-dimanche | 10h,16h dimanche | LLM |
| recrutement-rapport-lundi | 9h lundi | LLM |
| recrutement-hebdo | 9h lundi | LLM |
| prospection-mensuelle | 10h 1er du mois | no_agent |
| bilan-comptable-mensuel | 9h 1er du mois | no_agent |
| backup-hebdomadaire | 6h lundi | no_agent |
| prix-gazole | 7h lun-ven | no_agent |
| veille-produits-afrique | 10h samedi | no_agent |
| Rapport mensuel Veille & Marché | 9h 1er du mois | LLM |

## Messaging
- **Christine** — via email → check-email → Telegram (parle à hermes-tl)
- **Rémy** — via email → check-email → Telegram (parle à hermes-tl)
- **Flow** — via Telegram (quand sujet TL)

## Allowed Work
- Devis, factures, livraisons, stock, CMR
- Recrutement, prospection
- Email catia@transportlogic.fr
- Veille métier (gazole, concurrence, aides, lois)

## Forbidden Work
- Trading / crypto
- Création contenu TikTok (sauf vidéos commandées à hermes-content)
- Devops / infra

## Owner
- Flow (décisions finales)
- Christine (quotidien)
