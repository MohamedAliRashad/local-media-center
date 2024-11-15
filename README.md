# Local Media Center

A self-hosted, Docker-powered media center for users who want full control over their media libraries. This setup leverages various applications to manage, organize, and access movies, TV shows, and other media, with automated download, subtitle management, and a sleek frontend interface.

## Table of Contents
- [Overview](#overview)
- [Features](#features)
- [Installation](#installation)
- [Configuration](#configuration)
- [Available Services](#available-services)
- [Accessing the Applications](#accessing-the-applications)
- [License](#license)

## Overview

This media center environment includes popular applications such as Jellyfin, Radarr, Sonarr, and others for managing and accessing media collections. Nginx is configured as a reverse proxy to simplify access to each service via easy-to-remember routes.

## Features

- **Media Streaming**: Stream movies, TV shows, and other media with Jellyfin.
- **Automated Media Management**: Use Radarr, Sonarr, and other tools to automatically download, organize, and tag media files.
- **Subtitle Management**: Bazarr is configured for automatic subtitle downloads.
- **Torrent and Usenet Support**: qBittorrent and Sabnzbd offer flexible download options.
- **Single Access Point**: Access all services via a single IP address with customizable endpoints.

## Installation

### Prerequisites
- Docker and Docker Compose installed
- NVIDIA GPU (optional for hardware acceleration with Jellyfin)
- Environment variables set up (place these in a `.env` file in the root of this project):
  ```plaintext
  PUID=1000
  PGID=1000
  TZ=Europe/London
  MEDIA_CENTER_PATH=/path/to/your/media_center
  ```

### Steps
1. **Clone this repository**:
   ```bash
   git clone https://github.com/your-username/local-media-center.git
   cd local-media-center
   ```

2. **Update Environment Variables**:
   Modify the `.env` file to match your system's configuration.

3. **Run Docker Compose**:
   Start the containers using Docker Compose:
   ```bash
   docker-compose up -d
   ```

4. **Access the Services**:
   Open your browser and navigate to `http://your-server-ip` (or use Nginx-configured paths to access each service).

## Configuration

### Reverse Proxy (Nginx)
Nginx is configured to serve each application at a specific path (e.g., `/jellyfin`, `/radarr`). The `nginx.conf` file in this repository defines these paths and ensures smooth navigation across all services.

### Volumes and Paths
The following paths should be configured to match your media and configuration locations:
- **Media Storage**: `${MEDIA_CENTER_PATH}/media/movies`, `${MEDIA_CENTER_PATH}/media/tv`
- **Configuration Files**: `${MEDIA_CENTER_PATH}/config/{service_name}`
- **Download Directory**: `${MEDIA_CENTER_PATH}/downloads`

## Available Services

Each service runs in its own container and can be accessed through the Nginx reverse proxy:

| Service       | Path         | Port | Description                                |
|---------------|--------------|------|--------------------------------------------|
| Jellyfin      | `/jellyfin`  | 8096 | Media server for streaming media           |
| Jellyseerr    | `/jellyseerr`| 5055 | Request management for Jellyfin            |
| Radarr        | `/radarr`    | 7878 | Manages movie downloads and organization   |
| Sonarr        | `/sonarr`    | 8989 | Manages TV show downloads and organization |
| Prowlarr      | `/prowlarr`  | 9696 | Integrates with indexers for Radarr/Sonarr |
| Bazarr        | `/bazarr`    | 6767 | Automated subtitle management              |
| Homarr        | `/`          | 7575 | Customizable homepage for all services     |
| qBittorrent   | `/qbittorrent`| 5080| Torrent client for managing downloads      |
| Sabnzbd       | `/sabnzbd`   | 8080 | Usenet client for managing downloads       |

### Optional Services
Commented-out services in `docker-compose.yml` include:
- **Tdarr**: For video transcoding and optimization (requires an NVIDIA GPU).
- **Ngrok**: Expose the server securely via the web for remote access.

## Accessing the Applications

Each service is accessible via Nginx using predefined routes. Access each application by appending its path to the server’s IP address or hostname:

- **Example**: `http://your-server-ip/jellyfin` for Jellyfin
- **Homarr Homepage**: A landing page for all services, accessible at `http://your-server-ip/`

## License

This project is licensed under the MIT License. See the [LICENSE](./LICENSE) file for more information.
