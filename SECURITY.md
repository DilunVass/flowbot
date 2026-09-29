# Security policy

## Reporting a vulnerability

Please **don't** open a public issue for security problems.

Report them privately through GitHub: go to the **Security** tab of this repository and choose **Report a vulnerability**. You can also email [SECURITY EMAIL].

Include:
- the Flowbot version (`FLOWBOT_VERSION` in your `.env`)
- steps to reproduce
- what an attacker could do with it

You'll get a reply within 5 working days. Fixes are released as a new version, and the changelog notes them once users have had time to upgrade.

## Supported versions

Security fixes go into the latest minor release. Please stay on a current version.

## What Flowbot does to protect you

- Runs entirely on your server, with no telemetry
- Stores passwords as bcrypt hashes
- Limits sign-in attempts per client (`RATE_LIMIT_LOGIN`)
- Runs as an unprivileged user inside the container
- Signs each release image, so you can verify it came from the official build (see the README)

## What you are responsible for

- Keeping `.env` private (`chmod 600 .env`). It holds the owner password, `JWT_SECRET` and your OpenAI key
- Using a long random `JWT_SECRET` and a strong `OWNER_PASSWORD`
- Serving Flowbot over HTTPS (see [docs/https.md](docs/https.md))
- Restricting `CORS_ORIGINS` to your own websites
- Keeping the host, Docker and Flowbot up to date
