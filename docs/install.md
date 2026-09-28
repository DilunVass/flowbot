# Install Flowbot

This takes about 15 minutes on a fresh server.

## 1. Prepare the server

You need a Linux server with:

- Docker 24 or newer and Docker Compose v2 ([install guide](https://docs.docker.com/engine/install/))
- At least 2 CPU cores, 4 GB RAM, and 20 GB free disk
- Port 8080 open (or port 443 once you set up HTTPS)

Check Docker is ready:

```bash
docker --version
docker compose version
```

## 2. Download the install files

```bash
curl -fsSLO https://github.com/YOUR_GITHUB_NAME/flowbot/releases/latest/download/flowbot-install.zip
unzip flowbot-install.zip
cd flowbot
```

You now have `docker-compose.yml`, `.env.example`, and an `examples` folder.

## 3. Create your settings file

```bash
cp .env.example .env
chmod 600 .env
```

Generate a secret key:

```bash
docker run --rm ghcr.io/YOUR_GITHUB_NAME/flowbot:v1.0.0 generate-secret
```

Open `.env` (for example with `nano .env`) and set at least:

- `FLOWBOT_SECRET_KEY`: the value you just generated. **Save a copy somewhere safe.**
- `DB_PASSWORD`: a long random password.
- `FLOWBOT_PUBLIC_URL`: the address people will use, such as `http://your-server-ip:8080`.

Everything else can stay as is for now. See [configuration.md](configuration.md) for all settings.

## 4. Start Flowbot

```bash
docker compose up -d
```

The first start downloads the images and prepares the database, which can take a few minutes. Check that everything is running:

```bash
docker compose ps
docker compose exec app flowbot check-config
```

## 5. Finish setup in the browser

Open `FLOWBOT_PUBLIC_URL` in your browser. The setup screen asks you to:

1. Create the admin account.
2. Connect an AI provider, or skip this to use rules only. See [llm-providers.md](llm-providers.md).
3. Enter SMTP settings for handoff emails, if you didn't put them in `.env`.

## 6. Build and publish your first bot

1. Create a chatbot and add a welcome node.
2. Add keyword rules for common questions.
3. Optionally upload documents to the knowledge base.
4. Click **Preview** to test it.
5. Copy the embed code from **Settings** and paste it before `</body>` on your website.

## Optional: run AI models locally

To keep everything on your server, start the bundled Ollama service:

```bash
docker compose --profile local-ai up -d
```

The first run downloads the models (several GB). Then set in `.env`:

```bash
FLOWBOT_LOCAL_BASE_URL=http://ollama:11434/v1
FLOWBOT_EMBEDDING_MODEL=local/bge-m3
FLOWBOT_CHAT_MODEL=local/qwen2.5:7b
```

and run `docker compose --profile local-ai up -d` again. A local chat model needs a GPU to answer quickly; see [llm-providers.md](llm-providers.md).

## Next steps

- [Put Flowbot behind HTTPS](https.md). Do this before adding the widget to a public website.
- [Set up backups](backup.md).
