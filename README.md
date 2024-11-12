# File: README.md
# Bitfocus Companion Docker for ARM64

Docker image for Bitfocus Companion running on ARM64 devices (like Raspberry Pi).

## Quick Start
```bash
mkdir companion && cd companion
docker compose up -d
```

Access the web interface at:
- Local: http://localhost:8000
- Network: http://YOUR-PI-IP:8000

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

## Support
For issues, please create a ticket in the GitHub repository.

# File: docker-compose.yml
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

# File: .gitignore
data/
config/
*.log

# File: LICENSE
MIT License

Copyright (c) 2024 bob2187

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
