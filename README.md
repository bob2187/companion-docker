# Bitfocus Companion Docker for ARM64

Docker image for Bitfocus Companion running on ARM64 devices (like Raspberry Pi).

## Quick Start
```bash
docker pull bob2187/companion-arm64:latest
mkdir companion && cd companion
curl -O https://raw.githubusercontent.com/bob2187/companion-docker/main/docker-compose.yml
docker compose up -d
```

## Features
- ARM64 native support
- Headless operation
- USB device support for Stream Deck
- Persistent configuration
- Automatic restart on boot

## Requirements
- ARM64 device (like Raspberry Pi 4/5)
- Docker and Docker Compose installed
- USB devices accessible (for StreamDeck support)

## Installation
1. Create a directory for Companion:
```bash
mkdir companion && cd companion
```

2. Download the docker-compose.yml:
```bash
curl -O https://raw.githubusercontent.com/bob2187/companion-docker/main/docker-compose.yml
```

3. Start Companion:
```bash
docker compose up -d
```

## Configuration
The container uses several volumes for persistence:
- `./data`: Data directory
- `./config`: Configuration files
- `/dev/bus/usb`: USB device access (for Stream Deck)

## Access
Web interface available at:
- Local: http://localhost:8000
- Network: http://YOUR-PI-IP:8000

## docker-compose.yml
```yaml
version: '3.8'

services:
  companion:
    image: bob2187/companion-arm64:latest
    container_name: companion
    restart: unless-stopped
    network_mode: "host"
    privileged: true
    volumes:
      - ./data:/data
      - ./config:/root/.config/companion-nodejs/v3.4
      - /dev/bus/usb:/dev/bus/usb
      - /run/dbus:/run/dbus
    environment:
      - COMPANION_DEBUG=1
      - ELECTRON_ENABLE_LOGGING=1
      - COMPANION_HEADLESS=1
      - COMPANION_NO_GUI=1
    devices:
      - "/dev/bus/usb:/dev/bus/usb"
```

## Troubleshooting
If you encounter issues:

1. Check container logs:
```bash
docker compose logs -f
```

2. Verify USB devices are accessible:
```bash
lsusb
```

3. Check network access:
```bash
curl http://localhost:8000
```

4. Restart the container:
```bash
docker compose restart
```

5. Rebuild from scratch:
```bash
docker compose down
docker compose up -d
```

## Support
For issues, please create a ticket in the GitHub repository.

## License
MIT License

## Credits
- Built on [Bitfocus Companion](https://bitfocus.io/companion)
- Containerized for ARM64 devices by bob2187