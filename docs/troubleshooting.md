# Troubleshooting

Start with these two commands. They answer most questions:

```bash
docker compose ps
docker compose logs --tail=100 flowbot
```

## Flowbot doesn't start

**"validation error for Settings" in the logs**: a required setting is missing or invalid. The lines after it name the setting, for example `owner_email` or `jwt_secret`. Fix it in `.env` and run `docker compose up -d`.

**"owner_password ... at least 8 characters"**: choose a longer `OWNER_PASSWORD`.

**Port already in use**: change `FLOWBOT_PORT` in `.env`, then run `docker compose up -d`.

## I changed `.env` but nothing happened

Run `docker compose up -d`. `docker compose restart` doesn't load new settings.

## I can't sign in

- Sign in with `OWNER_EMAIL` and `OWNER_PASSWORD` from `.env`.
- To reset the password, change `OWNER_PASSWORD` and run `docker compose up -d`.
- After 5 failed attempts in a minute, sign-in is paused for that minute (`RATE_LIMIT_LOGIN`).

## Typed questions fail with "unavailable"

A model didn't answer. The logs have a line starting `LLM request failed on` that says why:

- **"LLM_MODEL_PATH points at ... which does not exist"**: download the model into `models/model.gguf` (see [models/README.md](../models/README.md)).
- **"... is not a GGUF file"**: the download was interrupted. Delete the file and download it again.
- **"RAG_ENABLED is on but OPENAI_API_KEY is not set"**: add the key, or set `RAG_ENABLED=false`.
- **A connection or timeout error**: with `LLM_MODE=http`, check that Ollama is running and reachable from the container (see [llm-providers.md](llm-providers.md#ollama)). Otherwise, check that the server can reach `api.openai.com`.

The first message after a start is slower while the model loads.

## Documents fail to process

- Processing needs `OPENAI_API_KEY`. Check it's set, then use **Retry** on the document.
- Supported files are PDF, TXT, Markdown and HTML, up to 10 MB. Scanned PDFs without a text layer have no text to extract.
- A document is retried at most 3 times. After that, fix the file and add it again.

## The bot hands off too often

- Add nodes to your flows for the questions in the handoff queue, or accept the dashboard's suggested nodes.
- Add documents that cover those topics.
- Lower `RAG_SCORE_THRESHOLD` slightly (for example to `0.7`). Lower values allow more document answers, but also more weak ones.

## My website can't reach the chat API

- Your site uses HTTPS but Flowbot doesn't. Set up [HTTPS](https.md).
- Your site isn't in `CORS_ORIGINS`. Add it, for example `["https://www.example.com"]`, and run `docker compose up -d`.
- Check that the API key is active and the chatbot hasn't been deactivated.
- Open the browser console (F12) and look for the failing request.

## Still stuck

[Open an issue](https://github.com/DilunVass/flowbot/issues/new/choose) with your version (`FLOWBOT_VERSION`) and the recent logs. Remove keys, passwords and email addresses first.
