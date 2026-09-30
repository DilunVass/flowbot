# Backup and restore

Back up two things:

1. **The `flowbot-data` volume**: the SQLite database (projects, chatbots, flows, conversations, the knowledge base and its search index) and the original uploaded documents.
2. **Your `.env` file**: the owner account settings, `JWT_SECRET`, and your OpenAI key.

The model file in `models/` can always be downloaded again.

## Back up

Stop Flowbot briefly so the database isn't being written while it's copied.

On Linux or macOS:

```bash
mkdir -p backups
docker compose stop
docker run --rm -v flowbot_flowbot-data:/data -v "$PWD/backups":/backup alpine \
  tar czf /backup/flowbot-data-$(date +%F).tar.gz -C /data .
docker compose start
cp .env backups/env-$(date +%F).bak
```

On Windows, in PowerShell:

```powershell
New-Item -ItemType Directory -Force backups | Out-Null
$date = Get-Date -Format yyyy-MM-dd
docker compose stop
docker run --rm -v flowbot_flowbot-data:/data -v "${PWD}/backups:/backup" alpine tar czf "/backup/flowbot-data-$date.tar.gz" -C /data .
docker compose start
Copy-Item .env "backups/env-$date.bak"
```

Copy the `backups` folder to another machine or storage. To run it daily, save the commands above as `backup.sh` in the Flowbot folder and add a cron entry, for example:

```
0 2 * * * cd /opt/flowbot && ./backup.sh
```

On Windows, save the PowerShell commands as `backup.ps1` and schedule it with Task Scheduler.

## Restore

Replace `2026-01-15` with the date of the backup you want. On Linux or macOS:

```bash
docker compose down
docker run --rm -v flowbot_flowbot-data:/data -v "$PWD/backups":/backup alpine \
  sh -c "rm -rf /data/* && tar xzf /backup/flowbot-data-2026-01-15.tar.gz -C /data"
docker compose up -d
```

On Windows, in PowerShell:

```powershell
docker compose down
docker run --rm -v flowbot_flowbot-data:/data -v "${PWD}/backups:/backup" alpine sh -c "rm -rf /data/* && tar xzf /backup/flowbot-data-2026-01-15.tar.gz -C /data"
docker compose up -d
```

A backup restores on any system, so you can use this to move Flowbot from your computer to a server. Copy your `.env` across too.

## Warning

`docker compose down -v` deletes the `flowbot-data` volume, which holds all of Flowbot's data. Never use `-v` unless you mean to erase everything.
