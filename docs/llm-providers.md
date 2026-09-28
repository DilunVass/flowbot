# AI providers

Flowbot doesn't include an AI service. You connect your own, and pay the provider directly for what you use. Rules-only chatbots need no provider at all.

## Two kinds of model

- **Chat model**: writes answers from your documents.
- **Embedding model**: turns documents and questions into numbers so Flowbot can find relevant passages.

They can come from different providers, for example Google embeddings with Anthropic for chat.

## Adding a key

**In the dashboard (recommended):** go to **Settings → AI providers → Add provider**, choose the provider, paste the key, and click **Test**. Keys are encrypted before being saved and are only shown masked afterwards.

**In `.env`:** set the matching `FLOWBOT_*_API_KEY` value and run `docker compose up -d`. Keys set this way show as "Managed by server configuration" in the dashboard.

## What leaves your server

- To the **embedding** provider: document text when documents are processed, and each visitor question that reaches the document tier.
- To the **chat** provider: the visitor question and the matching document passages.

Nothing is sent if the rules answer the question. Nothing leaves your server at all if you use local models.

## Local models with Ollama

```bash
docker compose --profile local-ai up -d
```

In `.env`:

```bash
FLOWBOT_LOCAL_BASE_URL=http://ollama:11434/v1
FLOWBOT_EMBEDDING_MODEL=local/bge-m3
FLOWBOT_CHAT_MODEL=local/qwen2.5:7b
```

Hardware guide:

| Use | Needs |
|---|---|
| Local embeddings only | Normal CPU, 8 GB+ RAM |
| Local chat model (7–8B) | NVIDIA GPU with ~16 GB memory for responsive answers. On CPU, answers can take 10+ seconds |

Check the license of any model you download; most recommended ones allow commercial use, but not all do.

## Changing the embedding model

Embeddings from different models can't be mixed. When you change the embedding model, Flowbot re-processes every document in the background and keeps answering with the old model until it's done. It shows the number of documents first. With a paid provider, this costs tokens.

## Common errors

| Message | Meaning |
|---|---|
| "Key rejected" | Key is wrong, revoked, or for another provider |
| "No remaining credit" | Add credit or a payment method with the provider |
| "Provider is rate limiting" | Too many requests; answers are slower until it clears |
| "Can't reach provider" | Server has no outbound internet access or a firewall blocks it |
