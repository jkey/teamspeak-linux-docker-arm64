# TeamSpeak 3 Server for ARM64

[![Docker Build and Publish](https://github.com/jkey/teamspeak-linux-docker-arm64/actions/workflows/docker-publish.yml/badge.svg)](https://github.com/jkey/teamspeak-linux-docker-arm64/actions/workflows/docker-publish.yml)

This is a Docker image for TeamSpeak 3 Server that runs on ARM64 architecture (e.g., Raspberry Pi 4/5, Apple Silicon, AWS Graviton). The x86_64 TeamSpeak server binaries are run using [Box64](https://github.com/ptitSeb/box64) emulation.

## 🚀 Quick Start

Pull and run the latest image from GitHub Container Registry:

```bash
docker pull ghcr.io/jkey/teamspeak-linux-docker-arm64:latest

docker run -d \
  --name teamspeak \
  -p 9987:9987/udp \
  -p 10011:10011 \
  -p 30033:30033 \
  -v /path/to/data:/var/ts3server \
  ghcr.io/jkey/teamspeak-linux-docker-arm64:latest
```

## 📦 Available Tags

Images are automatically tagged with the TeamSpeak version from the Dockerfile:

- `latest` - Latest stable build from main branch
- `3.13.7` - Specific TeamSpeak version (automatically extracted from Dockerfile)
- `main` - Latest build from main branch (may be unstable)
- `<sha>` - Specific commit SHA for reproducible builds

**Example: Pin to a specific TeamSpeak version:**
```bash
docker pull ghcr.io/jkey/teamspeak-linux-docker-arm64:3.13.7
```

## 🔧 Configuration

### Environment Variables

The container supports all standard TeamSpeak 3 Server environment variables:

```bash
docker run -d \
  --name teamspeak \
  -p 9987:9987/udp \
  -p 10011:10011 \
  -p 30033:30033 \
  -e TS3SERVER_LICENSEPATH=/var/ts3server/license \
  -e TS3SERVER_QUERY_PROTOCOLS=raw,ssh \
  -e TS3SERVER_SERVERADMIN_PASSWORD=your_password \
  -v /path/to/data:/var/ts3server \
  ghcr.io/jkey/teamspeak-linux-docker-arm64:latest
```

### Ports

- **9987/udp** - Default voice port
- **10011/tcp** - Server query port
- **30033/tcp** - File transfer port

### Volumes

- `/var/ts3server` - Server data directory (database, logs, etc.)

## 🏗️ Building Locally

```bash
cd debian
docker build -t teamspeak-arm64 .
```

## 🔄 Automatic Updates

This repository includes automated workflows:

- **Automatic Docker builds** on every push to main
- **Daily version checks** for new TeamSpeak releases (08:00 UTC)
- **Automatic Pull Requests** when new TeamSpeak versions are available

## 📋 System Requirements

- ARM64 architecture (aarch64)
- Docker or compatible container runtime
- At least 512MB RAM
- Persistent storage for data directory

## 🛠️ Technical Details

- **Base Image**: arm64v8/debian:bookworm-slim
- **Emulation**: Box64 for running x86_64 binaries
- **TeamSpeak Version**: 3.13.7 (automatically updated)

## 📄 License

This Docker image uses TeamSpeak 3 Server software which is subject to TeamSpeak's license agreement.

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.
