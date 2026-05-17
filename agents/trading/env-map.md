# hermes-trading - Env / Secret Map

## Keys

| Key | Purpose | Stored where |
|---|---|---|
| `OPENROUTER_API_KEY` | LLM inference | `profiles/trading/.env` |
| `COINBASE_API_KEY` | Exchange data | `profiles/trading/.env` |

## Rules
- No real trading without validation.
- Wallets never stored in `.env`.
