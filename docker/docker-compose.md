# Docker Compose Cheat Sheet

## What Docker Compose Is

Docker Compose is a tool for running **multi-container applications** with one config file, usually `compose.yml`.

Instead of starting each container manually with long `docker run` commands, you define:

- services (app, db, cache, etc.)
- networks
- volumes
- environment variables

Then manage the whole stack with `docker compose ...` commands.

---

## Minimal `compose.yml` Example

```yaml
services:
  web:
    build: .
    ports:
      - "3000:3000"
    environment:
      NODE_ENV: development
    depends_on:
      - db
    volumes:
      - .:/app

  db:
    image: postgres:16
    environment:
      POSTGRES_DB: appdb
      POSTGRES_USER: app
      POSTGRES_PASSWORD: secret
    volumes:
      - pgdata:/var/lib/postgresql/data

volumes:
  pgdata:
```

---

## Common Commands

### Start / Stop

- `docker compose up` - Start all services
- `docker compose up -d` - Start in detached mode (background)
- `docker compose down` - Stop and remove containers + default network
- `docker compose down -v` - Also remove named volumes
- `docker compose stop` - Stop services without removing containers
- `docker compose start` - Start previously stopped services

### Build / Rebuild

- `docker compose build` - Build images for services with `build:`
- `docker compose up --build` - Build then start
- `docker compose build --no-cache` - Rebuild from scratch
- `docker compose pull` - Pull newer images from registry

### Logs / Status / Inspection

- `docker compose ps` - Show service/container status
- `docker compose logs` - Show logs for all services
- `docker compose logs -f` - Follow logs in real time
- `docker compose logs -f web` - Follow logs for one service
- `docker compose top` - Running processes per service
- `docker compose config` - Validate + render merged config

### Execute Commands in Containers

- `docker compose exec web sh` - Open shell in running `web` container
- `docker compose exec db psql -U app appdb` - Run command in `db`
- `docker compose run --rm web npm test` - One-off command with temporary container

### Scale

- `docker compose up -d --scale web=3` - Run multiple replicas of `web`

---

## Day-to-Day Workflows

### First run

1. Create `compose.yml`
2. Run `docker compose up --build`
3. Visit app on mapped port (for example `localhost:3000`)

### Normal development loop

1. `docker compose up -d`
2. `docker compose logs -f web`
3. `docker compose exec web sh` for debugging
4. `docker compose down` when done

### Reset everything (destructive)

```bash
docker compose down -v --remove-orphans
docker compose up --build
```

---

## Useful File Fields

- `services` - App components (web, api, db, redis)
- `image` - Prebuilt image to run
- `build` - Build instructions from local Dockerfile
- `ports` - Host:container port mapping (`"8080:80"`)
- `environment` - Environment variables
- `volumes` - Persistent data or bind mounts
- `depends_on` - Startup order hint between services
- `networks` - Custom service connectivity rules

---

## Override Files

Common pattern:

- `compose.yml` for shared/default config
- `compose.override.yml` for local dev overrides (auto-loaded)
- `compose.prod.yml` for production settings

Use multiple files explicitly:

```bash
docker compose -f compose.yml -f compose.prod.yml up -d
```

---

## Troubleshooting Quick Hits

- Port already in use:
  - change host port in `ports`
  - or stop conflicting process/container
- Container exits immediately:
  - check `docker compose logs <service>`
  - verify env vars and startup command
- Changes not reflected:
  - rebuild image (`docker compose up --build`)
  - verify bind mount paths
- "Orphan containers" warning:
  - run `docker compose down --remove-orphans`
- Need a clean state:
  - `docker compose down -v` (removes named volumes)

---

## Legacy Note

- Modern syntax: `docker compose ...` (Docker Compose v2 plugin)
- Older syntax: `docker-compose ...` (v1 standalone, mostly deprecated)
