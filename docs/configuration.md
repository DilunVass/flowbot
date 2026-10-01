# Configuration

All settings live in `.env` next to `docker-compose.yml`.

> After changing `.env`, run `docker compose up -d`. A plain `docker compose restart` keeps the old values.

If a value contains a `$` sign, wrap it in single quotes: `OWNER_PASSWORD='pa$$word'`.

## Server

| Setting | Required | Default | Description |
|---|---|---|---|
| `FLOWBOT_VERSION` | | `v1.0.0` | Image version to run |
| `FLOWBOT_PORT` | | `8000` | Port on the server. Use `127.0.0.1:8000` behind a reverse proxy |
| `PUBLIC_BASE_URL` | yes | | Address people reach Flowbot on. Download links for uploaded files use it |
| `FRONTEND_BASE_URL` | yes | | Same value as `PUBLIC_BASE_URL`. Links in invitations use it |
| `CORS_ORIGINS` | | `["*"]` | Websites allowed to call the chat API from a browser, as a JSON list. The dashboard doesn't need an entry |

## Owner account

| Setting | Required | Description |
|---|---|---|
| `OWNER_EMAIL` | yes | Sign-in email of the account that owns this installation |
| `OWNER_PASSWORD` | yes | 8 to 128 characters |
| `OWNER_FULL_NAME` | | Display name |

The owner account is created on first start and brought back in line with these values on every start. To reset a forgotten password, change `OWNER_PASSWORD` and run `docker compose up -d`. Changing `OWNER_EMAIL` creates a second owner account; it doesn't rename the first.

## Email

| Setting | Required | Description |
|---|---|---|
| `GMAIL_USER` | | Gmail address that invitations and handoff notices are sent from |
| `APP_PASSWORD` | | An app password for that account, not its normal password |

Both are optional, but without them no email is sent, so the people you invite never get their invitation link. Set these before you invite teammates.

To create an app password, turn on 2-Step Verification for the Google account, then open [App passwords](https://myaccount.google.com/apppasswords). Gmail is the only supported mail service for now.

## Sign-in tokens

| Setting | Required | Default | Description |
|---|---|---|---|
| `JWT_SECRET` | yes | | Signs sign-in tokens. Generate with `openssl rand -hex 32`. Changing it signs everyone out |
| `JWT_ACCESS_TOKEN_EXPIRE_MINUTES` | | `60` | How long a sign-in lasts |

## Local routing model

| Setting | Default | Description |
|---|---|---|
| `LLM_MODE` | `in_process` | `in_process` runs a GGUF file inside Flowbot; `http` uses an OpenAI-compatible server such as Ollama |
| `LLM_MODEL_PATH` | `llm_models/model.gguf` | The model file. `llm_models/` is the `models` folder beside `docker-compose.yml` |
| `LLM_CONTEXT_TOKENS` | `4096` | Context window. Raise it if a chatbot has many nodes; it costs memory |
| `LLM_THREADS` | automatic | CPU threads for `in_process` |
| `LLM_MAX_ANSWER_TOKENS` | `48` | Generation limit for a routing decision |
| `LLM_BASE_URL` | | Server address for `http` mode, such as `http://host.docker.internal:11434/v1` |
| `LLM_MODEL` | `qwen3:0.6b` | Model name for `http` mode |
| `LLM_TIMEOUT_SECONDS` | `30` | How long to wait for the server in `http` mode |

See [llm-providers.md](llm-providers.md).

## Knowledge tier

OpenAI is the only supported provider for the knowledge tier for now. Support for other providers is planned.

| Setting | Default | Description |
|---|---|---|
| `RAG_ENABLED` | `false` | Answer from your documents when no flow node fits |
| `OPENAI_API_KEY` | | Needed when `RAG_ENABLED=true`, and to process documents |
| `RAG_BASE_URL` | `https://api.openai.com/v1` | OpenAI-compatible endpoint |
| `RAG_MODEL` | `gpt-4o-mini` | Model that writes answers |
| `RAG_EMBEDDING_MODEL` | `text-embedding-3-small` | Model that indexes documents |
| `RAG_EMBEDDING_DIMENSIONS` | `1536` | Must match the embedding model |
| `RAG_SCORE_THRESHOLD` | `0.75` | How closely a document passage must match (0–1) before it's used. Raise it for fewer, safer answers |
| `RAG_TOP_K` | `5` | Passages given to the model per question |
| `RAG_ANSWER_SMALLTALK` | `true` | Reply to greetings and thanks instead of handing them off |
| `SUGGEST_NODES` | `true` | Suggest new flow nodes for questions the documents keep answering |

Changing the embedding model or its dimensions means re-adding every document, because vectors from different models can't be compared.

## Limits

| Setting | Default | Description |
|---|---|---|
| `RATE_LIMIT_DEFAULT` | `100/minute` | Requests per client for most of the API |
| `RATE_LIMIT_LOGIN` | `5/minute` | Sign-in attempts per client |

Per-project and per-chatbot usage limits are set in the dashboard.

## Leave as they are

| Setting | Value | Description |
|---|---|---|
| `APP_NAME` | `Flowbot` | Name the API reports |
| `DEBUG` | `false` | |
| `ENVIRONMENT` | `production` | `development` also serves interactive API docs at `/docs` |
| `DATABASE_PATH` | `data/flowbot.db` | Inside the `flowbot-data` volume |
| `FILE_STORAGE_DIR` | `data/uploads` | Inside the `flowbot-data` volume |
