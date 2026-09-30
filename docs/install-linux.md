# Install Flowbot on Linux

This takes about 10 minutes on a fresh server. Installing on a Mac or Windows PC instead? See [install.md](install.md).

## 1. Prepare the server

You need a Linux server (x86-64) with:

- Docker 24 or newer and Docker Compose v2 ([install guide](https://docs.docker.com/engine/install/))
- At least 2 CPU cores, 2 GB RAM and 5 GB free disk
- Port 8000 open (or port 443 once you set up HTTPS)

Check that Docker is ready:

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

It's about 640 MB. To use Ollama instead, skip this step and see [llm-providers.md](llm-providers.md).

## 4. Create your settings file

```bash
cp .env.example .env
chmod 600 .env
```

Generate a secret for sign-in tokens:

```bash
openssl rand -hex 32
```

Open `.env` (for example with `nano .env`) and set:

- `OWNER_EMAIL` and `OWNER_PASSWORD`: the account you'll sign in with. The password needs at least 8 characters.
- `JWT_SECRET`: the value you just generated.
- `PUBLIC_BASE_URL` and `FRONTEND_BASE_URL`: the address people will use, such as `http://your-server-ip:8000`.

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

## 6. Sign in and build your first bot

Open `PUBLIC_BASE_URL` in your browser and sign in with the owner account. Then follow the setup steps on the dashboard:

1. Create a project.
2. Create a chatbot and choose its mode: flows only, documents only, or hybrid.
3. Add a flow with a few nodes, or add documents to the knowledge base.
4. Try it in the builder's test panel.
5. Create an API key and connect your website. The dashboard's widget guide shows how.

## Next steps

- [Put Flowbot behind HTTPS](https.md). Do this before connecting a public website.
- [Set up backups](backup.md).
- To answer from your documents, [enable the knowledge tier](llm-providers.md#knowledge-tier-openai).
