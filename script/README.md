# TeslaMate Deployment Scripts

[← Back to Main README](../README.md)

This directory contains Docker Compose configurations for deploying TeslaMate.

## Files

- `docker-compose.yml` - Official TeslaMate version (international users)
- `docker-compose-with-mytesla.yml` - Mytesla optimized version (recommended for China)
- `images/` - Dashboard screenshots

## Quick Deployment

### Option 1: Official Version (International Users)
Uses official TeslaMate and Grafana images.

```bash
docker compose up -d
```

**Services included:**
- TeslaMate web interface (official)
- PostgreSQL database
- Grafana dashboards (official)
- TeslaMateAPI (for mobile apps like Mytess)
- Mosquitto MQTT broker

**Best for**: Users outside mainland China, or those preferring official images.

### Option 2: Mytesla Optimized Version (Recommended for China)
Uses Mytesla-optimized images with China-specific fixes.

```bash
docker compose -f docker-compose-with-mytesla.yml up -d
```

**Additional features:**
- OpenStreetMap proxy (solves map loading in China)
- Baidu Maps geocoding support
- Chinese-localized Grafana dashboards
- Traefik reverse proxy with unified authentication
- Modern Mytesla Dash web interface
- All services from official version

**Best for**: Users in mainland China experiencing map loading issues.

## Configuration

### Required Environment Variables

**Important:** Change these before production deployment!

```yaml
# Authentication (Mytesla Dash version)
AUTH_USERNAME=admin              # Change this!
AUTH_PASSWORD=admin123           # Change this!
SECRET_KEY=change-me-random-32   # Change this!

# TeslaMate encryption
ENCRYPTION_KEY=change-me-random  # Change this!

# Database
POSTGRES_PASSWORD=teslamate      # Change this!

# API Token (for Mytess app)
API_TOKEN=change-me-token        # Change this!
```

### Generate Secure Random Keys

Use OpenSSL to generate secure random strings:

```bash
openssl rand -base64 32
```

This will output a 32-character random string. Use this for `ENCRYPTION_KEY`, `SECRET_KEY`, and `API_TOKEN`.

### Important Notes

- **ENCRYPTION_KEY**: Used to encrypt sensitive data like Tesla API tokens. If you lose this key, you cannot recover your data!
- **API_TOKEN**: Used by Mytess app to authenticate with TeslaMateAPI. Must match between config and app.
- **Database Password**: Change from default to prevent unauthorized access.

## Architecture

### Basic Version

```
┌─────────────────────────────────────────────────────────────┐
│                    Your Network                              │
│                                                              │
│  ┌──────────┐   ┌──────────┐   ┌──────────┐                │
│  │TeslaMate │   │ Grafana  │   │   API    │                │
│  │  :4000   │   │  :3000   │   │  :3030   │                │
│  └────┬─────┘   └────┬─────┘   └────┬─────┘                │
│       │              │              │                        │
│       └──────────────┼──────────────┘                        │
│                      │                                       │
│              ┌───────▼────────┐                              │
│              │   PostgreSQL   │                              │
│              └────────────────┘                              │
└─────────────────────────────────────────────────────────────┘
```

### Mytesla Dash Version

```
┌─────────────────────────────────────────────────────────────┐
│                    Your Network                              │
│                                                              │
│                    ┌──────────┐                              │
│                    │ Traefik  │ :80 (Unified Entry)          │
│                    └────┬─────┘                              │
│                         │                                    │
│         ┌───────────────┼───────────────┐                    │
│         │               │               │                    │
│    ┌────▼─────┐   ┌────▼────┐   ┌──────▼─────┐              │
│    │TeslaMate │   │  Dash   │   │    API     │              │
│    │/teslamate│   │    /    │   │/mytesla/api│              │
│    └────┬─────┘   └────┬────┘   └──────┬─────┘              │
│         │              │               │                     │
│         └──────────────┼───────────────┘                     │
│                        │                                     │
│                ┌───────▼────────┐                            │
│                │   PostgreSQL   │                            │
│                └────────────────┘                            │
│                                                              │
│         ┌──────────────┐                                     │
│         │ Auth Service │ (ForwardAuth)                       │
│         └──────────────┘                                     │
└─────────────────────────────────────────────────────────────┘
```

## Access Points

### Basic Version
- **TeslaMate**: `http://your-ip:4000`
- **Grafana**: `http://your-ip:3000` (default: admin/admin)
- **TeslaMateAPI**: `http://your-ip:3030`

### Mytesla Dash Version
- **Unified entry**: `http://your-ip` (redirects to Dashboard)
- **Dashboard**: `http://your-ip/`
- **TeslaMate**: `http://your-ip/teslamate`
- **Grafana**: `http://your-ip/grafana`
- **TeslaMateAPI**: `http://your-ip/mytesla/api` (no auth required)
- **Settings**: `http://your-ip/settings` (change auth credentials)
- **Default login**: `admin` / `admin123`

## Connecting Mytess iOS App

### Step 1: Note Your TeslaMateAPI Endpoint

- **Basic version**: `http://your-server-ip:3030`
- **Mytesla Dash version**: `http://your-server-ip/mytesla/api`

Replace `your-server-ip` with:
- Your local IP (e.g., `192.168.1.100`) for home network access
- Your public IP or domain name for internet access

### Step 2: Open Mytess App

1. Download Mytess from [App Store](https://apps.apple.com/app/id6757828502)
2. Open the app
3. Go to **Settings** → **Server Configuration**

### Step 3: Enter Server Information

1. **Server URL**: Enter your TeslaMateAPI endpoint from Step 1
2. **API Token**: Enter the `API_TOKEN` value from your docker-compose file
3. Tap **Test Connection**
4. If successful, you'll see your vehicle data

### Troubleshooting Connection

- **Connection timeout**: Check firewall allows port 3030 (basic) or 80 (Mytesla Dash)
- **401 Unauthorized**: API_TOKEN mismatch - ensure it matches your config
- **404 Not Found**: Check URL format - must include `http://` prefix

For detailed Mytess setup, visit [mytess.net](https://mytess.net).

## Troubleshooting

### Services Not Starting

Check logs to diagnose issues:

```bash
# View all service logs
docker compose logs

# View specific service logs
docker compose logs teslamate
docker compose logs database

# Follow logs in real-time
docker compose logs -f
```

Restart services:

```bash
# Restart all services
docker compose restart

# Restart specific service
docker compose restart teslamate
```

### Cannot Connect from Mobile

**Check firewall:**
```bash
# Allow port 3030 (basic version)
sudo ufw allow 3030

# Allow port 80 (Mytesla Dash version)
sudo ufw allow 80
```

**Verify services are running:**
```bash
docker compose ps
```

**Test connectivity:**
- From same network: Try accessing from web browser first
- From internet: Ensure router port forwarding is configured

### Map Not Loading

- Built-in proxy included for China users to access OpenStreetMap
- Mytesla Dash version includes Baidu Maps support for better China coverage
- Check TeslaMate logs if map still doesn't load: `docker compose logs teslamate`

### Database Connection Failed

**Check database is running:**
```bash
docker compose ps database
```

**Check database password matches:**
- `POSTGRES_PASSWORD` in database service
- `DATABASE_PASS` in teslamate service
- Must be identical!

**Reset database (WARNING: deletes all data):**
```bash
docker compose down -v  # Remove volumes
docker compose up -d    # Start fresh
```

### Grafana Dashboard Missing Data

**Check Grafana can connect to database:**
```bash
docker compose logs grafana
```

**Verify database credentials match:**
- `DATABASE_USER`, `DATABASE_PASS`, `DATABASE_NAME`, `DATABASE_HOST` in grafana service
- Should match database service configuration

## Maintenance

### Backup Database

```bash
docker compose exec database pg_dump -U teslamate teslamate > backup.sql
```

### Restore Database

```bash
docker compose exec -T database psql -U teslamate teslamate < backup.sql
```

### Update Services

```bash
# Pull latest images
docker compose pull

# Restart with new images
docker compose up -d
```

### View Resource Usage

```bash
docker compose stats
```

## Documentation

- **Main README**: [../README.md](../README.md)
- **Chinese Guide**: [../README.zh.md](../README.zh.md)
- **TeslaMate Official Docs**: [docs.teslamate.org](https://docs.teslamate.org)
- **Mytess Website**: [mytess.net](https://mytess.net)

## Support

- **Discord**: [https://discord.com/invite/2DBzQfFPW8](https://discord.com/invite/2DBzQfFPW8)
- **Email**: hi@mytesla.cc
- **GitHub Issues**: [Report a problem](https://github.com/gococonut/teslamate-cn-image/issues)
