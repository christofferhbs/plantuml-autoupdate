# PlantUML Server with auto-updates

## Use case

Run a local PlantUML server with automatic daily updates.

## Features

- Automatic daily updates via Watchtower (scheduled for 02:00 UTC)
- Automatic restart on container failure

## Quick start

```bash
docker-compose up -d        # or: docker compose up -d
```

The server will be available at: <http://localhost:8081/plantuml>

Watchtower monitors the PlantUML server image daily and automatically deploys updates at 02:00 UTC.

> Note: Docker Desktop is not required. The setup supports any Docker Engine accessible to the `docker` CLI (including WSL-based or alternative distributions), provided Docker Compose v2 is available (`docker compose` or `docker-compose`) and the daemon API version is ≥ 1.44.

## Configuration file

Add shared PlantUML settings in `config.txt` (mounted read-only at `/config/config.txt` inside the container). The content is prepended to every diagram request, mirroring the `-config` CLI flag.

## Customization

Edit `docker-compose.yml` to customize:

- **Port**: Change `8081:8080` under `ports` to use a different host port
- **Base path**: Modify `BASE_URL=/plantuml` under `environment`
- **Other settings**: Adjust cache size, security limits, etc. under `environment`

After editing, restart: `docker-compose down && docker-compose up -d`

## Management commands

```bash
# Start server
docker-compose up -d

# Stop and remove containers
docker-compose down

# View status
docker-compose ps

# View logs
docker-compose logs -f

# Restart server
docker-compose restart

# Rebuild (if you modify docker-compose.yml)
docker-compose down
docker-compose up -d
```

## Troubleshooting

### Docker daemon not running

- Windows: Start Docker Desktop and wait for initialization to complete
- Linux: Ensure the Docker service is running, e.g. `sudo systemctl start docker`

### Port 8081 already in use

Edit `docker-compose.yml` and change the port mapping under the `plantuml` service:

```yaml
ports:
  - "9000:8080"  # Changed from 8081 to 9000
```

Then restart: `docker-compose down && docker-compose up -d`
