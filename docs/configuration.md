# Configuration

All settings live in `.env` next to `docker-compose.yml`.

> After changing `.env`, run `docker compose up -d`. A plain `docker compose restart` keeps the old values.

## General

| Setting | Required | Default | Description |
|---|---|---|---|
| `FLOWBOT_VERSION` | | `v1.0.0` | Image version to run |
| `FLOWBOT_PORT` | | `8080` | Port on the server |
| `FLOWBOT_PUBLIC_URL` | yes | | Address users reach Flowbot on; used in emails and embed code |
| `FLOWBOT_SECRET_KEY` | yes | | Encrypts stored API keys. Back it up |
| `FLOWBOT_LOG_LEVEL` | | `info` | `debug`, `info`, `warning`, `error` |

## Database

| Setting | Required | Default | Description |
|---|---|---|---|
| `DB_NAME` | | `flowbot` | Database name |
| `DB_USER` | | `flowbot` | Database user |
| `DB_PASSWORD` | yes | | Database password |

Changing `DB_PASSWORD` after the first start does not change the existing database's password. See [troubleshooting.md](troubleshooting.md).

## AI providers

| Setting | Description |
|---|---|
| `FLOWBOT_OPENAI_API_KEY` | OpenAI key |
| `FLOWBOT_GOOGLE_API_KEY` | Google Gemini key |
| `FLOWBOT_ANTHROPIC_API_KEY` | Anthropic key |
| `FLOWBOT_MISTRAL_API_KEY` | Mistral key |
| `FLOWBOT_AZURE_OPENAI_API_KEY`, `FLOWBOT_AZURE_OPENAI_ENDPOINT` | Azure OpenAI |
| `FLOWBOT_LOCAL_BASE_URL` | Any OpenAI-compatible server, such as `http://ollama:11434/v1` |
| `FLOWBOT_CHAT_MODEL` | Default chat model, as `provider/model` |
| `FLOWBOT_EMBEDDING_MODEL` | Default embedding model, as `provider/model` |
| `LOCAL_EMBEDDING_MODEL`, `LOCAL_CHAT_MODEL` | Models the bundled Ollama downloads |

All provider keys are optional and can be added in the dashboard instead.

## Answering behaviour

| Setting | Default | Description |
|---|---|---|
| `FLOWBOT_ENABLED_TIERS` | `rules,classifier,rag` | Tiers new chatbots may use. `rules` alone means no AI |
| `FLOWBOT_RAG_MIN_SCORE` | `0.75` | Minimum document match (0–1) before an AI answer is written. Raise it for fewer, safer answers |

## Email

| Setting | Default | Description |
|---|---|---|
| `SMTP_HOST` | | Mail server |
| `SMTP_PORT` | `587` | Mail server port |
| `SMTP_USER`, `SMTP_PASSWORD` | | Login |
| `SMTP_FROM` | | Sender shown on handoff emails |
| `SMTP_SECURITY` | `starttls` | `starttls`, `ssl`, or `none` |

## File storage

| Setting | Default | Description |
|---|---|---|
| `FLOWBOT_STORAGE` | `local` | `local` stores uploads in a Docker volume; `s3` uses S3-compatible storage |
| `S3_ENDPOINT`, `S3_BUCKET`, `S3_ACCESS_KEY`, `S3_SECRET_KEY`, `S3_REGION` | | Needed when `FLOWBOT_STORAGE=s3` |
