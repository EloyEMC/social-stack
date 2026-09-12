# Local development

This scaffold is for local testing only. It does not deploy to a VPS and contains no production credentials.

## Prerequisites

- Docker Desktop or another Docker Engine with Compose v2.
- Sufficient local disk space for the named volumes.

## First startup

1. Copy `.env.example` to `.env`.
2. Keep the placeholder values for an initial smoke test, or replace them with locally generated values.
3. Validate the configuration:

   ```sh
   docker compose --env-file .env config
   ```

4. Start the stack:

   ```sh
   docker compose --env-file .env up -d
   ```

5. Open Postiz at <http://localhost:5000> and n8n at <http://localhost:5678>.

The first startup may take several minutes while images are downloaded and Postiz initializes its database. The Compose file includes Postiz's PostgreSQL and Redis dependencies for local use.

## Shutdown and cleanup

Stop containers while preserving local data:

```sh
docker compose --env-file .env down
```

To remove the containers and their named volumes, use the explicit volume-removal form only when the local test data is disposable:

```sh
docker compose --env-file .env down -v
```

## Safety rules

- Never commit `.env`, OAuth tokens, API keys, Telegram bot tokens, or private media.
- Use a test Telegram bot and test social accounts only. Do not connect production accounts during scaffold validation.
- Keep Postiz and n8n bound to localhost unless an explicit, reviewed local-network test requires otherwise.
- Do not expose these services directly to the internet; HTTPS, authentication, backups, and secret management are deployment concerns.
- Restrict Telegram commands to an allowlist and require confirmation before public or destructive actions.
- Treat workflow exports as sensitive if they contain credential references or private content, even when credentials are stored separately.

## Known limitations

- Postiz is currently referenced with the `latest` image tag. Pin a validated Postiz version before relying on this setup or deploying anywhere.
- Postiz environment names, API behavior, OAuth callback requirements, and dependency requirements can change; verify them against the selected release documentation.
- The planned Telegram-to-Postiz workflow is documentation only. No Telegram trigger, OAuth connection, AI provider, or Postiz API credential is configured.
- Local HTTP URLs are suitable for smoke tests, but external Telegram webhooks and OAuth callbacks generally require a secure, reachable HTTPS endpoint and additional review.
