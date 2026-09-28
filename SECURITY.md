# Security policy

## Reporting a vulnerability

Please **don't** open a public issue for security problems.

Report them privately through GitHub: go to the **Security** tab of this repository and choose **Report a vulnerability**. You can also email [SECURITY EMAIL].

Include:
- the Flowbot version (`docker compose exec app flowbot version`)
- steps to reproduce
- what an attacker could do with it

You'll get a reply within 5 working days. Fixes are released as a new version and noted in the changelog once users have had time to upgrade.

## Supported versions

Security fixes go into the latest minor release. Please stay on a current version.

## What Flowbot does to protect you

- Runs entirely on your server; no telemetry
- API keys you enter in the dashboard are encrypted with `FLOWBOT_SECRET_KEY` before being stored
- Keys are never written to logs
- Each release image is signed and scanned for known vulnerabilities

## What you are responsible for

- Keeping `.env` private (`chmod 600 .env`) and backing up `FLOWBOT_SECRET_KEY`
- Serving Flowbot over HTTPS (see [docs/https.md](docs/https.md))
- Keeping the host, Docker, and Flowbot up to date
- Restricting access to the database port (it is not published by default)
