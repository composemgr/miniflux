# Miniflux

A self-hosted application for managing miniflux.

## Installation

### Option 1: Quick Install
```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/miniflux/main/docker-compose.yaml" | docker compose -f - up -d
```

### Option 2: Git Clone
```bash
git clone "https://github.com/composemgr/miniflux" ~/.local/srv/docker/miniflux
cd ~/.local/srv/docker/miniflux
docker compose up -d
```

### Option 3: Using composemgr
```bash
composemgr install miniflux
```

## Configuration

See docker-compose.yaml for environment variables and configuration options.

## Documentation

Check the official project documentation for detailed setup and usage information.
