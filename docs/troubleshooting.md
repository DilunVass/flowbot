# Troubleshooting

Start with these two commands. They answer most questions:

```bash
docker compose ps
docker compose exec app flowbot check-config
```

`check-config` tests the database, storage, AI providers, and email, without printing secrets.

## Flowbot doesn't start

**"FLOWBOT_SECRET_KEY is not set"**: create `.env` from `.env.example`, fill in the key, and run `docker compose up -d`.

**App keeps restarting**: read the logs with `docker compose logs --tail=100 app`.

**Port already in use**: change `FLOWBOT_PORT` in `.env`, then `docker compose up -d`.

## I changed `.env` but nothing happened

Run `docker compose up -d`. `docker compose restart` doesn't load new settings.

## Database password errors after changing `DB_PASSWORD`

The database keeps the password it was created with. Either set `DB_PASSWORD` back, or change it inside the database:

```bash
docker compose exec postgres psql -U flowbot -c "ALTER USER flowbot PASSWORD 'new-password';"
```

then update `.env` and run `docker compose up -d`.

## Documents stay "Processing"

- Check the worker is running: `docker compose ps worker`
- Read its logs: `docker compose logs --tail=100 worker`
- Check the AI provider in **Settings → AI providers** and click **Test**.

## The bot hands off too often

- Add keyword rules for the questions in your handoff emails.
- Upload documents that cover those topics.
- Lower `FLOWBOT_RAG_MIN_SCORE` slightly (for example to `0.7`). Lower values allow more AI answers but increase the risk of weak ones.

## The widget doesn't appear on my website

- Your site uses HTTPS but Flowbot doesn't: set up [HTTPS](https.md).
- Check `FLOWBOT_PUBLIC_URL` matches the address in the embed code.
- Check the chatbot is published, not in draft.
- Open the browser console (F12) and look for errors mentioning `embed.js`.

## Answers stop partway or arrive all at once

Your reverse proxy is buffering responses. Turn buffering off (see `proxy_buffering off` in `examples/nginx.conf`).

## Still stuck

[Open an issue](https://github.com/YOUR_GITHUB_NAME/flowbot/issues/new/choose) with your version, the `check-config` output, and recent logs. Remove keys and passwords first.
