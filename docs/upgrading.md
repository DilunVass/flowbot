# Upgrading

## Before you start

1. Read the **Upgrade notes** in [CHANGELOG.md](../CHANGELOG.md) for every version between yours and the new one.
2. [Take a backup](backup.md).

## Upgrade

1. Download the new release's install files and compare them with yours. New settings may have been added to `.env.example`:

   ```bash
   diff .env.example new/.env.example
   ```

2. Set the new version in `.env`:

   ```bash
   FLOWBOT_VERSION=v1.1.0
   ```

3. Pull and restart:

   ```bash
   docker compose pull
   docker compose up -d
   ```

Database changes apply automatically on startup. Check that it came back up:

```bash
docker compose ps
docker compose logs --tail=50 flowbot
```

## Rolling back

Set `FLOWBOT_VERSION` back to the previous version and run `docker compose up -d`. If the new version changed the database, restore the backup you took first.
