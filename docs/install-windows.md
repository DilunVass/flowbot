# Install Flowbot on Windows

This takes about 10 minutes. A Windows PC is a good place to try Flowbot or build your bots. To serve a public website, run it on a [Linux server](install-linux.md).

All commands below are for **PowerShell**. Open it from the Start menu.

## 1. Install Docker Desktop

Install [Docker Desktop for Windows](https://docs.docker.com/desktop/setup/install/windows-install/) and start it. Use the WSL 2 backend when the installer asks. You need:

- Windows 10 (22H2) or Windows 11, 64-bit, on an Intel or AMD processor
- Docker Desktop 4.25 or newer (it includes Docker Compose v2)
- At least 2 CPU cores and 2 GB RAM available to Docker
- 5 GB free disk

Check that Docker is ready:

```powershell
docker --version
docker compose version
```

## 2. Download the install files

```powershell
curl.exe -fsSLO https://github.com/DilunVass/flowbot/releases/latest/download/flowbot-install.zip
Expand-Archive flowbot-install.zip -DestinationPath .
cd flowbot
```

Type `curl.exe`, not `curl`: in PowerShell, plain `curl` runs a different command.

You now have `docker-compose.yml`, `.env.example`, a `models` folder and an `examples` folder.

## 3. Download the routing model

Flowbot uses a small local model to send typed questions to the right flow. Download it into `models\`:

```powershell
curl.exe -fL -o models\model.gguf https://huggingface.co/Qwen/Qwen3-0.6B-GGUF/resolve/main/Qwen3-0.6B-Q8_0.gguf
```

It's about 640 MB. To use Ollama instead, skip this step and see [llm-providers.md](llm-providers.md#ollama).

## 4. Create your settings file

```powershell
Copy-Item .env.example .env
```

Generate a secret for sign-in tokens:

```powershell
$bytes = New-Object byte[] 32
[Security.Cryptography.RandomNumberGenerator]::Create().GetBytes($bytes)
-join ($bytes | ForEach-Object { $_.ToString('x2') })
```

Open `.env` with `notepad .env` and set:

- `OWNER_EMAIL` and `OWNER_PASSWORD`: the account you'll sign in with. The password needs at least 8 characters.
- `JWT_SECRET`: the value you just generated.
- `PUBLIC_BASE_URL` and `FRONTEND_BASE_URL`: `http://localhost:8000`.

Save the file and close Notepad. Make sure it's still named `.env` and not `.env.txt`.

Everything else can stay as is for now. See [configuration.md](configuration.md) for every setting.

## 5. Start Flowbot

```powershell
docker compose up -d
```

The first start downloads the image and creates the database. Check that it's running and healthy:

```powershell
docker compose ps
docker compose logs --tail=50 flowbot
```

The log should end with `Application startup complete`.

Docker Desktop must be running for Flowbot to work. To start it automatically, turn on **Start Docker Desktop when you sign in to your computer** in Docker Desktop → Settings → General.

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
