# Flowbot

A self-hosted website chatbot that answers from your flows first, your documents second, and hands off to a person when it isn't sure.

Flowbot is **free to download and self-host**. The application is **closed source**: this repository contains the install files, documentation, and releases, and is where you can report issues. The application itself ships as a signed container image.

## How it answers

Every visitor message goes through up to four tiers, cheapest and most reliable first:

1. **Rules**: buttons and keyword rules you define. Instant and fully traceable.
2. **Smart routing** (optional): a small local model sends free-typed questions to the right flow.
3. **Your documents** (optional): answers grounded in files you upload, using the AI provider you choose.
4. **A real person**: if nothing above is confident, your team gets an email with the full conversation.

You can switch off the AI tiers per chatbot and run it fully rules-based.

## Features

- Visual flow builder and keyword rules with typo-tolerant matching
- Knowledge base from PDF, DOCX, TXT, Markdown, and web pages
- Bring your own AI provider: OpenAI, Google Gemini, Anthropic, Azure OpenAI, Mistral, or any OpenAI-compatible endpoint
- Fully offline mode with local models through Ollama
- One-line website embed
- Human handoff by email, plus webhooks
- Everything stays on your server; no telemetry

## Requirements

- Linux server with Docker 24+ and Docker Compose v2
- 2 CPU cores and 4 GB RAM (minimum, without local models)
- 20 GB free disk space
- For local AI models: 8 GB+ RAM for embeddings only; an NVIDIA GPU with ~16 GB memory for a local chat model

## Quick start

```bash
curl -fsSLO https://github.com/YOUR_GITHUB_NAME/flowbot/releases/latest/download/flowbot-install.zip
unzip flowbot-install.zip && cd flowbot
cp .env.example .env
chmod 600 .env
```

Generate a secret key and paste it into `FLOWBOT_SECRET_KEY` in `.env`, then set `DB_PASSWORD`:

```bash
docker run --rm ghcr.io/YOUR_GITHUB_NAME/flowbot:v1.0.0 generate-secret
```

Start Flowbot:

```bash
docker compose up -d
```

Open `http://your-server:8080` and follow the setup screen to create your admin account and connect an AI provider.

Full instructions: [docs/install.md](docs/install.md)

## Documentation

| Guide | What it covers |
|---|---|
| [Install](docs/install.md) | Step-by-step installation |
| [Configuration](docs/configuration.md) | Every `.env` setting |
| [AI providers](docs/llm-providers.md) | Adding API keys and local models |
| [HTTPS](docs/https.md) | Putting Flowbot behind HTTPS |
| [Upgrading](docs/upgrading.md) | Moving to a new version safely |
| [Backup and restore](docs/backup.md) | Protecting your data |
| [Troubleshooting](docs/troubleshooting.md) | Common problems and fixes |

## Verify the image

Every release is signed. To confirm an image came from the official build:

```bash
cosign verify ghcr.io/YOUR_GITHUB_NAME/flowbot:v1.0.0 \
  --certificate-identity-regexp 'https://github.com/YOUR_GITHUB_NAME/flowbot-core/.*' \
  --certificate-oidc-issuer https://token.actions.githubusercontent.com
```

## Your data and privacy

Flowbot runs entirely on your infrastructure. It makes network requests only to the AI providers you configure and the SMTP server you set for handoff emails. It does not send your documents, conversations, API keys, or usage data to the Flowbot author.

## Getting help

- Bugs and feature requests: [open an issue](../../issues/new/choose)
- Questions: [Discussions](../../discussions)
- Security problems: see [SECURITY.md](SECURITY.md), and please don't open a public issue

## License

Flowbot is free to use under the [Flowbot Free License](LICENSE). It includes open source components listed in [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).
