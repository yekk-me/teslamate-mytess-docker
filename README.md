# Mytess TeslaMate Deployment Scripts

[English](README.md) | [中文](README.zh.md)

> Quick deployment scripts for TeslaMate with Mytess integration - optimized for mainland China

## About Mytess

**Mytess** is a beautiful native iOS app that transforms your TeslaMate data into actionable insights on your iPhone.

<p align="center">
  <img src="https://www.mytess.net/screenshots/en/hero-1.png" width="300" alt="Mytess Dashboard">
  <img src="https://www.mytess.net/screenshots/en/hero-2.png" width="300" alt="Mytess Drive Insights">
</p>

### Key Features

#### Smart Drive Insights
Analyze your driving efficiency with "Golden Foot" scoring, safety distance analysis, and low temperature regeneration alerts.

<p align="center">
  <img src="https://www.mytess.net/screenshots/showcase/insights/en/91shots_so.png" width="250" alt="Drive Insights">
  <img src="https://www.mytess.net/screenshots/showcase/insights/en/181shots_so.png" width="250" alt="Drive Analysis">
</p>

#### Intelligent Charging Cost Calculation
Geofence-based automatic location identification with time-of-use (ToU) pricing support. Batch cost updates for historical data.

<p align="center">
  <img src="https://www.mytess.net/screenshots/showcase/costManagement/en/640shots_so.png" width="250" alt="Charging Costs">
  <img src="https://www.mytess.net/screenshots/showcase/costManagement/en/297shots_so.png" width="250" alt="Cost Management">
</p>

#### Interactive Map Mode
Visualize all drive routes on map with timeline selector for trip exploration and detailed elevation insights.

<p align="center">
  <img src="https://www.mytess.net/screenshots/showcase/mapMode/en/721shots_so.png" width="250" alt="Map Mode">
  <img src="https://www.mytess.net/screenshots/showcase/mapMode/en/258shots_so.png" width="250" alt="Route Visualization">
</p>

#### Real-time Notifications
Charging status alerts with Live Activity support on Dynamic Island and navigation sync.

<p align="center">
  <img src="https://www.mytess.net/screenshots/showcase/notifications/en/491shots_so.png" width="250" alt="Notifications">
  <img src="https://www.mytess.net/screenshots/showcase/notifications/en/94shots_so.png" width="250" alt="Live Activity">
</p>

#### Privacy First
All data stays on your own server with no third-party data sharing. Direct connection to your TeslaMate instance.

**Download:** [App Store](https://apps.apple.com/app/id6757828502) | **Website:** [mytess.net](https://mytess.net)

---

## About This Repository

This repository provides:
- Docker Compose configurations for TeslaMate deployment
- Pre-configured TeslaMateAPI for Mytess iOS app connectivity
- Optimized for mainland China (OpenStreetMap proxy, Baidu Maps)

## Why These Deployment Scripts?

- **China-Specific Optimizations**: Solves OpenStreetMap access issues and includes Baidu Maps support
- **Mytess-Ready**: Pre-configured TeslaMateAPI for seamless Mytess app connectivity
- **One-Command Deployment**: Get TeslaMate running in minutes

## Quick Start

### Option 1: Official TeslaMate (International Users)
Uses official TeslaMate images. Suitable for international users.

```bash
git clone https://github.com/gococonut/teslamate-cn-image.git
cd teslamate-cn-image/script
docker compose up -d
```

**Access points:**
- TeslaMate: `http://your-ip:4000`
- Grafana: `http://your-ip:3000`
- TeslaMateAPI: `http://your-ip:3030`

**Note**: May experience map loading issues in mainland China.

### Option 2: Mytesla Optimized Version (Recommended for China)
Uses Mytesla-optimized images with China-specific enhancements.

```bash
git clone https://github.com/gococonut/teslamate-cn-image.git
cd teslamate-cn-image/script
docker compose -f docker-compose-with-mytesla.yml up -d
```

**Access points:**
- Unified entry: `http://your-ip` (redirects to Dashboard)
- TeslaMate: `http://your-ip/teslamate`
- Grafana: `http://your-ip/grafana`
- TeslaMateAPI: `http://your-ip/mytesla/api`
- Default login: `admin` / `admin123`

**Mytesla version improvements**:
- Built-in OpenStreetMap proxy (solves map loading in China)
- Baidu Maps support for accurate geocoding
- Pre-configured Chinese Grafana dashboards
- Modern Mytesla Dash web interface
- Unified authentication (Traefik + ForwardAuth)

## Connecting Mytess to Your TeslaMate

1. **Find your TeslaMateAPI endpoint**
   - Basic version: `http://your-ip:3030`
   - Mytesla Dash version: `http://your-ip/mytesla/api`

2. **Open Mytess app**
   - Go to Settings → Server Configuration
   - Enter your server URL
   - Enter your API token (set in `API_TOKEN` environment variable)

3. **Test connection**
   - Tap "Test Connection"
   - If successful, you'll see your vehicle data

## Configuration

### Security Settings (MUST CHANGE)

Before production deployment, change these environment variables:

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

**Generate secure keys:**
```bash
openssl rand -base64 32
```

For detailed configuration instructions, see [script/README.md](script/README.md).

## Architecture

### Basic Version
```
┌─────────────┐
│  TeslaMate  │ ← Collects Tesla data
└──────┬──────┘
       │
┌──────▼──────┐
│ PostgreSQL  │ ← Stores data
└──────┬──────┘
       │
┌──────▼──────────┐
│ TeslaMateAPI    │ ← Provides API
└──────┬──────────┘
       │
┌──────▼──────┐
│   Mytess    │ ← iOS app
└─────────────┘
```

### Mytesla Dash Version
Includes Traefik reverse proxy for unified authentication and modern web dashboard.

## Troubleshooting

### Cannot access from mobile
- Check firewall allows required ports (80, 3030 for basic; 80 for Mytesla Dash)
- Verify server is accessible from mobile network
- Try accessing from browser first

### Mytess connection failed
- Verify TeslaMateAPI is running: `docker compose ps`
- Check API_TOKEN matches between config and Mytess app
- Ensure URL format is correct (include http://)

### Map not loading in TeslaMate
- Built-in proxy included for China users
- Mytesla Dash version includes Baidu Maps support

## Documentation

- **Full Deployment Guide**: [script/README.md](script/README.md)
- **Mytess Official**: [mytess.net](https://mytess.net)
- **TeslaMate Docs**: [docs.teslamate.org](https://docs.teslamate.org)

## Community & Support

- **Discord**: [https://discord.com/invite/2DBzQfFPW8](https://discord.com/invite/2DBzQfFPW8)
- **Email**: hi@mytesla.cc

## FAQ

**Q: Do I need to pay for these scripts?**
A: No, these deployment scripts are completely free. Mytess is a separate $9.9 iOS app available on the App Store.

**Q: What's the difference between Grafana and Mytesla Dash?**
A: Grafana provides detailed data visualization with customizable dashboards. Mytesla Dash is a modern, mobile-friendly alternative with unified authentication and simpler setup.

**Q: Does Mytesla Dash require a license?**
A: Yes, Mytesla Dash requires a one-time authorization. For licensing inquiries, please contact hi@mytesla.cc.

**Q: Can I use Mytess with my existing TeslaMate installation?**
A: Yes! You only need TeslaMateAPI running. See the [deployment guide](script/README.md) for adding TeslaMateAPI to existing setups.

**Q: Is my data secure?**
A: Yes! All data stays on your own server. Mytess connects directly to your TeslaMate instance - no third-party servers involved.

## Related Projects

- **TeslaMate**: [github.com/adriankumpf/teslamate](https://github.com/adriankumpf/teslamate)
- **TeslaMateAPI**: [github.com/tobiasehlert/teslamateapi](https://github.com/tobiasehlert/teslamateapi)
- **Mytess**: [mytess.net](https://mytess.net) - Native iOS app for TeslaMate
