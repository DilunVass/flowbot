# Install Flowbot on macOS

This takes about 10 minutes. A Mac is a good place to try Flowbot or build your bots. To serve a public website, run it on a [Linux server](install-linux.md).

## 1. Install Docker Desktop

Install [Docker Desktop for Mac](https://docs.docker.com/desktop/setup/install/mac-install/) and start it. You need:

- macOS with Docker Desktop 4.25 or newer (it includes Docker Compose v2)
- At least 2 CPU cores and 2 GB RAM available to Docker (Docker Desktop → Settings → Resources)
- 5 GB free disk

**Apple Silicon (M1 and later):** Flowbot's image is built for Intel (x86-64) processors, so Docker runs it under emulation. In Docker Desktop → Settings → General, turn on **Use Rosetta for x86_64/amd64 emulation on Apple Silicon**. Flowbot works this way, but it runs more slowly than on an Intel machine.

Open **Terminal** and check that Docker is ready:

```bash
docker --version
docker compose version
```

## 2. Download the install files

```bash
curl -fsSLO https://github.com/DilunVass/flowbot/releases/latest/download/flowbot-install.zip
unzip flowbot-install.zip
cd flowbot
```

You now have `docker-compose.yml`, `.env.example`, a `models` folder and an `examples` folder.

## 3. Download the routing model

Flowbot uses a small local model to send typed questions to the right flow. Download it into `models/`:

```bash
curl -fL -o models/model.gguf \
  https://huggingface.co/Qwen/Qwen3-0.6B-GGUF/resolve/main/Qwen3-0.6B-Q8_0.gguf
```

It's about 640 MB. To use Ollama instead, skip this step and see [llm-providers.md](llm-providers.md#ollama). On Apple Silicon, Ollama runs natively and answers faster than the emulated built-in model.

## 4. Create your settings file

```bash
cp .env.example .env
chmod 600 .env
```

Files starting with a dot are hidden in Finder. Press **Cmd+Shift+.** to show them.

Generate a secret for sign-in tokens:

```bash
openssl rand -hex 32
```

Open `.env` with `nano .env` and set:

- `OWNER_EMAIL` and `OWNER_PASSWORD`: the account you'll sign in with. The password needs at least 8 characters.
- `JWT_SECRET`: the value you just generated.
- `PUBLIC_BASE_URL` and `FRONTEND_BASE_URL`: `http://localhost:8000`.

In nano, press **Ctrl+O** then **Return** to save, and **Ctrl+X** to exit. Avoid TextEdit: it can turn quotes into curly quotes, which breaks the file.

Everything else can stay as is for now. See [configuration.md](configuration.md) for every setting.

## 5. Start Flowbot

```bash
docker compose up -d
```

The first start downloads the image and creates the database. Check that it's running and healthy:

```bash
docker compose ps
docker compose logs --tail=50 flowbot
```

The log should end with `Application startup complete`.

Docker Desktop must be running for Flowbot to work. To start it automatically, turn on **Start Docker Desktop when you sign in** in Docker Desktop → Settings → General.

## 6. Sign in and build your first bot

Open <http://localhost:8000> in your browser and sign in with the owner account. Then follow the setup steps on the dashboard:

1. Create a project.
2. Create a chatbot and choose its mode: flows only, documents only, or hybrid.
3. Add a flow with a few nodes, or add documents to the knowledge base.
4. Try it in the builder's test panel.
5. Create an API key and connect your website. The dashboard's widget guide shows how.

## Next steps

- [Set up backups](backup.md).
- To answer from your documents, [enable the knowledge tier](llm-providers.md#knowledge-tier-openai).
- Ready to go live? [Install on a Linux server](install-linux.md) and [put it behind HTTPS](https.md).
