# HTTPS

Serve Flowbot over HTTPS before adding the widget to a public website. Browsers block chat widgets loaded over plain HTTP on HTTPS sites.

## Option 1: Caddy (easiest)

Caddy gets and renews certificates automatically.

1. Point a domain (for example `chat.example.com`) to your server's IP address.
2. Install Caddy: https://caddyserver.com/docs/install
3. Copy `examples/Caddyfile` to `/etc/caddy/Caddyfile` and replace the domain.
4. Reload Caddy: `sudo systemctl reload caddy`
5. In `.env`, set `FLOWBOT_PUBLIC_URL=https://chat.example.com` and run `docker compose up -d`.

## Option 2: nginx

Use `examples/nginx.conf` with certificates from certbot or your organization. Keep `proxy_buffering off`, which streamed answers need.

## Option 3: Your existing load balancer

Forward traffic to port 8080 on the Flowbot server, and pass the `X-Forwarded-Proto` header. Disable response buffering for streamed answers.

## After enabling HTTPS

Close port 8080 to the internet so Flowbot is only reachable through HTTPS:

```bash
FLOWBOT_PORT=127.0.0.1:8080
```

Then run `docker compose up -d`.
