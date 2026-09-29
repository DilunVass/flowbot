# Backup and restore

Back up two things:

1. **The `flowbot-data` volume**: the SQLite database (projects, chatbots, flows, conversations, the knowledge base and its search index) and the original uploaded documents.
2. **Your `.env` file**: the owner account settings, `JWT_SECRET`, and your OpenAI key.

The model file in `models/` can always be downloaded again.

## Back up

Stop Flowbot briefly so the database isn't being written while it's copied:

```bash
mkdir -p backups
docker compose stop
docker run --rm -v flowbot_flowbot-data:/data -v "$PWD/backups":/backup alpine \
  tar czf /backup/flowbot-data-$(date +%F).tar.gz -C /data .
docker compose start
cp .env backups/env-$(date +%F).bak
```

Copy the `backups` folder to another machine or storage. To run it daily, save the commands above as `backup.sh` in the Flowbot folder and add a cron entry, for example:

```
0 2 * * * cd /opt/flowbot && ./backup.sh
```

## Restore

```bash
docker compose down
docker run --rm -v flowbot_flowbot-data:/data -v "$PWD/backups":/backup alpine \
  sh -c "rm -rf /data/* && tar xzf /backup/flowbot-data-2026-01-15.tar.gz -C /data"
docker compose up -d
```

## Warning

`docker compose down -v` deletes the `flowbot-data` volume, which holds all of Flowbot's data. Never use `-v` unless you mean to erase everything.
