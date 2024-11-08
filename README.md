# Docker Compose for \*arr Suite

A convenient `docker-compose.yml` for quickly setting up a working \*arr Suite with [Emby](https://emby.media/), [Homarr](https://homarr.dev/), [Transmission](https://transmissionbt.com/), [Radarr](https://radarr.video/), [Sonarr](https://sonarr.tv/), and [Prowlarr](https://prowlarr.com/).

The configuration is designed to be easily imported into [Portainer](https://www.portainer.io/) and can run seamlessly with dynamic media management services.

## Stack

-   **Emby** - Media server for organizing and streaming media content.
-   **Homarr** - Customizable homepage for easy access to your services.
-   **Transmission** - Torrent client, available with or without [VPN support](#additional).
-   **Radarr** - Automatic movie download manager.
-   **Sonarr** - Automatic TV series download manager.
-   **Prowlarr** - Indexer manager for Radarr and Sonarr.

## Setup

### Prerequisite

Folder structure for `${DATA_PATH}`:

```
${DATA_PATH}
├── downloads/
├── emby/
├── homarr/
├── movies/
├── prowlarr/
├── radarr/
├── sonarr/
├── transmission/
├── tv/
└── vpn/
```

You can create the following folders using the following command:

```bash
mkdir -p "${DATA_PATH}/"{downloads,emby,homarr,movies,prowlarr,radarr,sonarr,transmission,tv,vpn}
```

### Portainer

1. Go to **Stacks** > **Add Stack**.
2. Select **Repository** under **Build method**.
3. Paste `https://github.com/mradigen/arrsuite-docker` as the **Repository URL**.
4. Copy `.env.example` and adjust the environment variables if needed.
5. Deploy the stack.

### Or setup manually:

1. Clone this repository:

    ```bash
    git clone https://github.com/mradigen/arrsuite-docker.git
    cd arrsuite-docker
    ```

2. Create a `.env` file by copying the provided template:

    ```bash
    cp .env.example .env
    ```

3. Edit the `.env` file to configure your settings:

    - **UID/GID**: Set these to match your server user (default is `1000`).
    - **DATA_PATH**: Set this to the path where configuration and media files will be stored.
    - **Ports**: Define port mappings for each service if the defaults are occupied.
    - **Transmission VPN**: If using VPN, specify the OpenVPN configuration. [See here](#additional)

4. (Optional) If using Flood-UI for Transmission, download it from [flood-for-transmission](https://github.com/johman10/flood-for-transmission) and set the `TRANSMISSION_WEB_HOME` variable.

### Starting the Setup

To launch the services, run:

```bash
docker-compose up -d
```

This will pull the necessary images and start each service in the background.

## Accessing Services

Each service is accessible at `http://<your-server-ip>:<PORT>`:

-   **Emby**: Port `${PORT}` (default: 8096)
-   **Homarr**: Port `${PORT_HOMARR}` (default: 7575)
-   **Transmission**: Port `${PORT_FLOOD}` (default: 9091)
-   **Radarr**: Port `${PORT_RADARR}` (default: 7878)
-   **Sonarr**: Port `${PORT_SONARR}` (default: 8989)
-   **Prowlarr**: Port `${PORT_PROWLARR}` (default: 9696)

## Additional

-   **OpenVPN Configuration**: To use Transmission with VPN, uncomment the `transmission-openvpn` service in `docker-compose.yml`. Add your `.ovpn` file in `${DATA_PATH}/vpn` and update `OPENVPN_CONFIG` in `.env` to match the filename (without the `.ovpn` extension).
-   **Device Access**: Uncomment `runtime: nvidia` and `devices` in `docker-compose.yml` if your Emby server requires GPU acceleration.

## Contributing

Feel free to submit issues and pull requests for improvements or additional services.
