# MetaTrader5 Docker Setup for Proxmox

This guide provides instructions for deploying a MetaTrader5 Docker container in a Proxmox environment using Docker Compose with the `gmag11/metatrader5_vnc` image.

## Prerequisites

- Proxmox VE installed and configured
- Docker and Docker Compose installed on your Proxmox host or LXC container

## Setup Instructions

1. Clone this repository or copy the following files to your Proxmox host:
   - `docker-compose.yml`
   - `.env`

2. Configure the `.env` file:
   - Set the `VNC_PORT` if you want to use a different port (default is 5900)
   - Set the `TIMEZONE` to your preferred timezone (default is UTC)
   - Set the `MT5_DATA_PATH` to specify where MetaTrader5 data should be stored
   - Set `VNC_PASSWORD` for secure remote access (required)
   - Optionally, set MetaTrader5 account credentials (`MT5_LOGIN`, `MT5_PASSWORD`, `MT5_SERVER`)
   - Optionally, set additional MetaTrader5 parameters (`MT5_PARAMS`)

3. Deploy the container using Docker Compose:
   ```
   docker-compose up -d
   ```

4. Access the MetaTrader5 interface:
   - Use a VNC client to connect to `your_proxmox_host_ip:VNC_PORT`
   - Enter the VNC password you set in the `.env` file

## Additional Configuration

- The `docker-compose.yml` file is already configured to use the `gmag11/metatrader5_vnc:latest` image
- MetaTrader5 data is stored in the path specified by `MT5_DATA_PATH` (default: `./mt5_data`)
- A `config` volume is used for persistent configuration storage

## Volume Mappings

The `docker-compose.yml` file includes the following volume mappings:
```yaml
volumes:
  - ${MT5_DATA_PATH:-./mt5_data}:/home/wineuser/.wine/drive_c/Program Files/MetaTrader 5/MQL5
  - config:/home/wineuser/.wine/drive_c/Program Files/MetaTrader 5
```

To mount additional physical folders, add them to the `volumes` section in `docker-compose.yml`.

## Troubleshooting

- If you encounter issues, check the container logs:
  ```
  docker-compose logs metatrader5
  ```

- Ensure that the necessary ports (especially the VNC port) are open in your Proxmox firewall settings.

## Security Considerations

- Always use a strong password for VNC access (set in the `VNC_PASSWORD` environment variable)
- Consider using a VPN or SSH tunnel for added security when accessing the container remotely

For more information on using MetaTrader5, refer to the official documentation.
