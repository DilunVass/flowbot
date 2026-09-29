# Flowbot

A self-hosted website chatbot that answers from the flows you write first, your documents second, and hands the conversation to your team when it can't answer.

Flowbot is **free to download and self-host**. The application is **closed source**: this repository contains the install files, documentation and releases, and is where you can report issues. The application itself ships as a signed container image.

## How it answers

Every visitor message goes through these tiers in order, cheapest and most predictable first:

1. **Menus and rules**: buttons the visitor presses, and example phrasings you write. Instant, no AI involved.
2. **Smart routing**: a small local model reads free-typed questions and sends each one to the right node in your flows. It runs on your server, so this tier costs nothing per message.
3. **Your documents** (optional): answers grounded in files, web pages and text you add, written by OpenAI. It only answers from your own content.
4. **Your team**: when nothing above can answer, the conversation lands in the dashboard's handoff queue.

Each chatbot picks a mode: **flows only** (free and fully scripted), **documents only**, or **hybrid**.

## Features

- Flow builder with menus, buttons and example phrasings
- Local model for routing, running in-process or through Ollama
- Knowledge base from PDF, TXT, Markdown and HTML files, web pages, and pasted text
- Suggested new flow nodes, based on the questions your documents keep answering
- Handoff queue for conversations the bot can't answer
- Projects with team members, API keys, and usage limits per project and chatbot
- Usage statistics showing which tier answered each message
- Everything stays on your server, with a single SQLite database and no telemetry

## Requirements

- Linux server with Docker 24+ and Docker Compose v2 (x86-64/amd64)
- 2 CPU cores and 2 GB RAM
- 5 GB free disk space

## Quick start

```bash
curl -fsSLO https://github.com/DilunVass/flowbot/releases/latest/download/flowbot-install.zip
unzip flowbot-install.zip && cd flowbot
cp .env.example .env && chmod 600 .env
curl -fL -o models/model.gguf \
  https://huggingface.co/Qwen/Qwen3-0.6B-GGUF/resolve/main/Qwen3-0.6B-Q8_0.gguf
```

In `.env`, set `OWNER_EMAIL`, `OWNER_PASSWORD`, and a `JWT_SECRET` from `openssl rand -hex 32`. Then start Flowbot:

```bash
docker compose up -d
```

Open `http://your-server:8000` and sign in with the owner account.

Full instructions: [docs/install.md](docs/install.md)

## Documentation

| Guide | What it covers |
|---|---|
| [Install](docs/install.md) | Step-by-step installation |
| [Configuration](docs/configuration.md) | Every `.env` setting |
| [AI models](docs/llm-providers.md) | The local routing model and the OpenAI knowledge tier |
| [HTTPS](docs/https.md) | Putting Flowbot behind HTTPS |
| [Upgrading](docs/upgrading.md) | Moving to a new version safely |
| [Backup and restore](docs/backup.md) | Protecting your data |
| [Troubleshooting](docs/troubleshooting.md) | Common problems and fixes |

## Verify the image

Every release is signed by the build that produced it. To confirm an image is an official build that hasn't been changed:

```bash
cosign verify ghcr.io/dilunvass/flowbot:v1.0.0 \
  --certificate-identity-regexp '^https://github.com/DilunVass/flowbot-core/\.github/workflows/release\.yml@refs/tags/v' \
  --certificate-oidc-issuer https://token.actions.githubusercontent.com
```

Install cosign from https://docs.sigstore.dev/cosign/system_config/installation/.

## Your data and privacy

Flowbot runs entirely on your infrastructure. It makes outbound requests only to OpenAI when you enable the knowledge tier, to an Ollama server if you point it at one, and to web pages you add to the knowledge base. It sends nothing to the Flowbot author.

## Getting help

- Bugs and feature requests: [open an issue](https://github.com/DilunVass/flowbot/issues/new/choose)
- Questions: [Discussions](https://github.com/DilunVass/flowbot/discussions)
- Security problems: see [SECURITY.md](SECURITY.md), and please don't open a public issue

## License

Flowbot is free to use under the [Flowbot Free License](LICENSE). It includes open source components listed in [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).
