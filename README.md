## 👋 Welcome to Miniflux 🚀

Minimalist and opinionated feed reader

## 📋 Description

Miniflux is a minimalist feed reader. It polls RSS and Atom feeds on a
schedule, keeps unread state per category and entry, and exposes a small web
UI plus a REST API and Go API. There is no JavaScript framework and no client
side rendering.

The container is stateless. Everything lives in PostgreSQL: subscriptions,
entries, categories and users. On an empty database the stack runs migrations
and creates the administrator account named by `ADMIN_USERNAME` and
`ADMIN_PASSWORD`, which is why those two values must be set before the first
start.

## 🚀 Services

- **app**: miniflux/miniflux:latest

### Infrastructure Components

- **db**: Postgres database

## 📦 Installation

### Option 1: Quick Install
```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/miniflux/main/docker-compose.yaml" -o compose.yml
```

### Option 2: Git Clone
```bash
git clone "https://github.com/composemgr/miniflux" ~/.local/srv/docker/miniflux
cd ~/.local/srv/docker/miniflux
cp default.env .env
docker compose --env-file .env up -d
```

### Option 3: Using composemgr
```bash
composemgr install miniflux
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
APP_ORG_NAME=Miniflux
DB_USER_NAME=miniflux
DB_USER_PASS=replace-with-a-random-password
DB_CREATE_DATABASE_NAME=miniflux
APP_ADMIN_USER=administrator
APP_ADMIN_PASS=replace-with-a-random-password
```

See `docker-compose.yaml` for complete list of configurable options.

### Environment Files

`default.env.sample` holds the full set of variables and is copied to `.env`
before starting the stack:

```bash
cp default.env .env
docker compose --env-file .env up -d
```

`app.env.sample` holds the app-specific overrides and is copied to `app.env`:

```bash
cp app.env.sample app.env
```

`composemgr up` applies `app.env` and `default.env` automatically. Raw
`docker compose` commands need `--env-file .env` to pick up the same values.

### Reverse Proxy

Three standalone compose files ship in this repo:

| File | Use |
|------|-----|
| `docker-compose.yaml` | Default, published on `172.17.0.1:64256` |
| `docker-compose.traefik.yaml` | Behind an existing external `traefik` network |
| `docker-compose.tunnel.yaml` | Behind an existing external `cloudflare` tunnel network |

```bash
docker compose -f docker-compose.traefik.yaml --env-file .env up -d
```

Only the `app` service carries the proxy labels and joins the external
network. The database stays on the internal project network and is never
published.

`BASE_URL` is what Miniflux uses to build links in emails, the API and RSS
output. The default file points it at the published host port; the two proxy
variants point it at `https://` plus the hostname.

## 🌐 Access

- **Web Interface**: http://172.17.0.1:64256

## 📂 Volumes

- `./volumes/db/postgres/miniflux` - Database data

The app container is stateless; all durable state lives in Postgres.

## 🔐 Security

- Change all default passwords before deploying to production
- `APP_ADMIN_PASS` is the password for the account Miniflux creates on an
  empty database. Set it before the first start, and change it in the UI
  afterwards
- The database is only reachable on the internal project network, do not
  publish it
- Use HTTPS via one of the reverse proxy variants in production; Miniflux
  authenticates with a bearer token over the API
- `CREATE_ADMIN` only fires against an empty database, so changing
  `ADMIN_PASSWORD` later does not rotate an existing account
- Regularly update Docker images for security patches
- Back up the database regularly

## 🔍 Logging

```shell
docker compose --env-file .env logs -f app
```

## 🛠️ Management

```bash
# Start services
docker compose --env-file .env up -d

# Stop services
docker compose --env-file .env down

# Update to latest images
docker compose --env-file .env pull && docker compose --env-file .env up -d

# View logs
docker compose --env-file .env logs -f

# Restart services
docker compose --env-file .env restart

# Refresh all subscriptions now
docker compose --env-file .env exec app /usr/bin/miniflux -refresh-feeds
```

## 📋 Requirements

- Docker Engine 20.10+
- Docker Compose V2+
- At least 512 MB of memory

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖  
🦄 composemgr: [Github](https://github.com/composemgr) 🦄
