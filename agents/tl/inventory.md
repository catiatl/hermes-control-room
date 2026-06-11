# hermes-tl — Inventory

## Role
Agent Transport Logic. Gère le métier : devis Pennylane, livraisons, stock, RH, prospection, email automatique, veille sectorielle.

## Where It Runs
- Host: ThinkCenter (localhost)
- Profile: `tl` (`~/.hermes/profiles/tl/`)
- Modèle: `deepseek/deepseek-v4-flash` (OpenRouter)
- Data dir: `~/tl/`

## Skills
| Skill | Usage |
|-------|-------|
| `transport-logic/logic-apis` | StockLogic, DriveLogic APIs |
| `transport-logic/tl-chiffrage` | Calcul prix transport (coûts réels : gasoil 25L/100km, péages classe 3, 35€/h, camion 150€/j) |
| `transport-logic/tl-devis` | Création devis formaté |
| `transport-logic/pennylane` | API Pennylane (devis uniquement) |
| `transport-logic/recrutement-transport-logic` | Sourcing profils candidats |

## Pennylane
- Token: `PENNYLANE_TOKEN` dans `~/.hermes/.env`
- Expire: 22 nov 2026
- Base URL: `https://app.pennylane.com/api/external/v2`
- Scope autorisé: **Devis uniquement** (pas factures, avoirs, écritures)
- Clients: FLEX-O HAUSSMAN (ID 1180196098048), Mme Pagenel (ID 1248641298432)
- Format: invoice_lines avec `raw_currency_unit_price` en string, `FR_200` = TVA 20%, `country_alpha2` pour adresse

## Email
- SMTP: `spin.o2switch.net:465` (SSL)
- Compte: `catia@transportlogic.fr`
- Check email auto: script `~/.hermes/scripts/check-email.py`
- Cron: */3 6-21, */30 22-5 semaine, */10 weekend
- Répond à: fliaigre@, rverna@, rates@, christine@
- Tracking: `~/.hermes/data/replied_emails.json`
- Modèle check: Ollama gemma:2b (oui/non), Deepseek V4 Flash (contexte+rédaction)

## Cron Jobs
- check-email (4 crons no_agent)
- fly-benin-scan (jeudi 20h)
- (les devis sont créés à la demande, pas en cron)

## Prospection
- Sources officielles: Pappers API, Sirene, BOAMP, Douanes
- Pipeline froid: devineur email + email direct

## Infos légales
- SARL Transport Logic
- Siège: 1 Rue Clément Ader, 95160 Montmorency
- RCS Pontoise 917 493 702
- TVA: FR00917493702
- Dirigeants: Rémy Verna, Florent Liaigre, Erwan Dossou