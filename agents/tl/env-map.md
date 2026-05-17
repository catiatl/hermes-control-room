# hermes-tl - Env / Secret Map

## Keys

| Key | Purpose | Stored where |
|---|---|---|
| `OPENROUTER_API_KEY` | LLM inference | `profiles/tl/.env` |
| `PENNYLANE_TOKEN` | API Pennylane (devis, factures) | `profiles/tl/.env` |
| `HUNTER_API_KEY` | Prospection email | `profiles/tl/.env` |
| `TAVILY_API_KEY` | Web search (prospection) | `profiles/tl/.env` |

## Rules
- Do not paste raw secret values in this file.
- Use per-profile `.env` files.
- Pennylane validation OBLIGATOIRE avant création.
