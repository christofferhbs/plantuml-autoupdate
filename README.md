# PlantUML Server with auto-updates

## Use case

Run a local PlantUML server with automatic daily updates.

## Features

- **Automatic daily updates** via Watchtower (scheduled for 02:00 UTC)
- **Automatic restart** enabled - containers restart on crashes and system reboots (`restart: always`)
- **Cleanup old images** after updates (`WATCHTOWER_CLEANUP=true`)
- **Monitor stopped containers** and restart them if needed
- **24-hour polling interval** for image updates (`WATCHTOWER_POLL_INTERVAL=86400`)

## Quick start

```bash
docker-compose up -d        # or: docker compose up -d
```

The server will be available at: <http://localhost:8081/plantuml>

Watchtower monitors the PlantUML server image daily and automatically deploys updates at 02:00 UTC. After updates, old images are cleaned up automatically, and containers are configured to restart on crashes and system reboots.

## Configuration file

Add shared PlantUML settings in `config.txt` (mounted read-only at `/config/config.txt` inside the container). The content is prepended to every diagram request, mirroring the `-config` CLI flag.

## Customization

Edit `docker-compose.yml` to customize:

- **Port**: Change `8081:8080` under `ports` to use a different host port
- **Base path**: Modify `BASE_URL=/plantuml` under `environment`
- **Update schedule**: Modify `WATCHTOWER_SCHEDULE=0 2 * * *` (default: 02:00 UTC daily)
- **Polling interval**: Adjust `WATCHTOWER_POLL_INTERVAL=86400` (seconds between checks)
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
