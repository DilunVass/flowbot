# HTTPS

Serve Flowbot over HTTPS before connecting it to a public website. Browsers block requests from an HTTPS page to a plain HTTP API, and sign-in details would otherwise cross the network unencrypted.

## Option 1: Caddy (easiest)

Caddy gets and renews certificates automatically.

1. Point a domain (for example `chat.example.com`) at your server's IP address.
2. Install Caddy: https://caddyserver.com/docs/install
3. Copy `examples/Caddyfile` to `/etc/caddy/Caddyfile` and replace the domain.
4. Reload Caddy: `sudo systemctl reload caddy`

## Option 2: nginx

Use `examples/nginx.conf` with certificates from certbot or your organization.

## Option 3: Your existing load balancer

Forward traffic to port 8000 on the Flowbot server, and pass the `X-Forwarded-For` and `X-Forwarded-Proto` headers. Allow request bodies of at least 10 MB for document uploads, and a read timeout of at least 60 seconds.

## After enabling HTTPS

Set both addresses to the HTTPS domain, and stop Flowbot's port from being reachable from the internet, so all traffic goes through the proxy. In `.env`:

```bash
PUBLIC_BASE_URL=https://chat.example.com
FRONTEND_BASE_URL=https://chat.example.com
FLOWBOT_PORT=127.0.0.1:8000
```

Then run `docker compose up -d`.
