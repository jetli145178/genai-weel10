# n8n Docker Deployment

This repository contains a Docker Compose deployment for n8n using Postgres and Redis for durability.

## Quick Start

1. Copy the example environment file:

   ```bash
   cp .env.example .env
   ```

2. Update `.env` with secure credentials.

3. Start the stack:

   ```bash
   docker compose up -d
   ```

4. Open n8n in your browser:

   ```text
   http://localhost:5678
   ```

## Stop the stack

```bash
docker compose down
```

To remove volumes as well:

```bash
docker compose down -v
```

## Services

- `n8n` - n8n workflow automation UI and runtime
- `postgres` - Postgres database for workflow and credential persistence
- `redis` - Redis for queueing and execution processing

## Notes

- The `.env` file contains secrets and should not be committed.
- Database state is persisted in Docker volumes:
  - `pgdata`
  - `redisdata`
  - `n8n_data`
- Use strong passwords for `N8N_BASIC_AUTH_PASSWORD` and `POSTGRES_PASSWORD`.
- For production, front n8n with a reverse proxy and TLS, and back up Postgres data regularly.

## Custom n8n image

This repository includes a `Dockerfile` at `n8n/Dockerfile` so you can build a custom n8n image that installs extra NPM packages or ships custom nodes.

To add custom nodes or packages:

1. Place node modules or custom node folders under `n8n/custom`.
2. Optionally set the build arg `NPM_PACKAGES` in your environment to install global npm packages during build.

Build and start the stack (compose will build the image):

```bash
docker compose up -d --build
```

The `n8n` service in `docker-compose.yml` is configured to build the image from `n8n/Dockerfile` and tag it `genai-weel10-n8n:local`.

## Verify Deployment

- Confirm services are running:

  ```bash
  docker compose ps
  ```

- Access the n8n UI at `http://localhost:5678`.
- Create a simple workflow and restart containers to verify persistence.
- Use `docker compose logs n8n` for troubleshooting.
