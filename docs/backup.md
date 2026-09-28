# Backup and restore

Back up three things:

1. **The database**: chatbots, flows, rules, conversations, and document search data.
2. **Uploaded files**: the original documents.
3. **Your `.env` file**, especially `FLOWBOT_SECRET_KEY`. Without it, saved API keys can't be decrypted.

## Back up

```bash
mkdir -p backups
docker compose exec -T postgres pg_dump -U flowbot -Fc flowbot > backups/db-$(date +%F).dump
docker run --rm -v flowbot_files:/data -v "$PWD/backups":/backup alpine \
  tar czf /backup/files-$(date +%F).tar.gz -C /data .
cp .env backups/env-$(date +%F).bak
```

Copy the `backups` folder to another machine or storage. Schedule it daily with cron, for example:

```
0 2 * * * cd /opt/flowbot && ./backup.sh
```

## Restore

```bash
docker compose up -d postgres
docker compose exec -T postgres pg_restore -U flowbot -d flowbot --clean < backups/db-2026-01-15.dump
docker run --rm -v flowbot_files:/data -v "$PWD/backups":/backup alpine \
  sh -c "rm -rf /data/* && tar xzf /backup/files-2026-01-15.tar.gz -C /data"
docker compose up -d
```

## Warning

`docker compose down -v` deletes all Flowbot data, including the database. Never use `-v` unless you mean to erase everything.
