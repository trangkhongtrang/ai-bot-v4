# MetaTrader 5 Docker Setup

![Banner](https://github.com/user-attachments/assets/6b5101ea-275b-4ae4-8f65-6a4fc30f30bf)

## Table of Contents

- [MetaTrader 5 Docker Setup](#metatrader-5-docker-setup)
  - [Table of Contents](#table-of-contents)
  - [Overview](#overview)
  - [Features](#features)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Configuration](#configuration)
    - [Environment Variables](#environment-variables)
    - [Docker Compose Services](#docker-compose-services)
    - [Volumes](#volumes)
  - [Usage](#usage)
  - [Logging](#logging)
  - [Troubleshooting](#troubleshooting)
  - [Contributing](#contributing)
  - [License](#license)

## Overview

This project provides a Docker-based setup to run MetaTrader 5 (MT5) using Wine on a Debian-based environment. It leverages Traefik as a reverse proxy for managing HTTP/HTTPS traffic and ensures secure access through Let's Encrypt certificates. The setup includes VNC for remote desktop access and is configured to run as a background service with proper logging and environment management.

## Features

- **Dockerized Environment:** Simplifies deployment and management of MT5.
- **Wine Compatibility:** Runs MetaTrader 5 on a Linux-based system.
- **Traefik Integration:** Handles reverse proxying with automatic SSL certificate generation via Let's Encrypt.
- **VNC Access:** Provides remote desktop access to the MT5 application.
- **Logging:** Implements structured logging for easy monitoring and debugging.
- **Environment Configuration:** Easily manage settings using environment variables.

## Prerequisites

- **Docker:** Ensure Docker is installed on your system. [Install Docker](https://docs.docker.com/get-docker/)
- **Docker Compose:** Required for orchestrating the services. [Install Docker Compose](https://docs.docker.com/compose/install/)
- **Domain Name:** A registered domain for accessing Traefik and VNC services.
- **SSL Certificate:** Managed automatically via Let's Encrypt.

## Installation

1. **Clone the Repository**

   ```bash
   git clone -b chapter-1 --single-branch https://github.com/sesto-dev/metatrader5-linux-django-docker.git
   cd metatrader5-linux-django-docker
   ```

2. **Configure Environment Variables**

   Create a `.env` file based on the provided example:

   ```bash
   cp .env .env
   ```

   Open the `.env` file and set the necessary variables:

   ```env
   # Backend - MT5
   CUSTOM_USER=admin
   PASSWORD=yourpassword
   VNC_DOMAIN=your-vnc-domain.com

   # Traefik
   TRAEFIK_DOMAIN=your-traefik-domain.com
   TRAEFIK_USERNAME=yourusername
   ACME_EMAIL=youremail@example.com
   ```

   **Note:** To generate and set the hashed password in one command for Traefik's HTTP Basic Auth, you can use the following command:

   ```bash
   export TRAEFIK_HASHED_PASSWORD=$(openssl passwd -apr1 $PASSWORD)
   ```

3. **Create Docker Network**

   ```bash
   docker network create traefik-public
   ```

4. **Build and Start the Services**

   ```bash
   docker-compose up -d
   ```

This command builds the Docker images and starts the services in detached mode.

## Configuration

### Environment Variables

- `CUSTOM_USER`: Username for accessing the MT5 service.
- `PASSWORD`: Password for the custom user.
- `VNC_DOMAIN`: Domain for accessing the VNC service.
- `TRAEFIK_DOMAIN`: Domain for Traefik dashboard.
- `TRAEFIK_USERNAME`: Username for Traefik basic authentication.
- `ACME_EMAIL`: Email address for Let's Encrypt notifications.

### Docker Compose Services

- **Traefik:** Acts as a reverse proxy with HTTPS support.
- **MT5:** Runs MetaTrader 5 using Wine.

### Volumes

- `/var/run/docker.sock`: Allows Traefik to monitor Docker services.
- `./config`: Stores Wine configurations and MT5 data.
- `traefik-public-certificates`: Persists SSL certificates generated by Let's Encrypt.

## Usage

1. **Accessing MetaTrader 5**

   Navigate to `https://your-vnc-domain.com` in your web browser to access the VNC interface for MetaTrader 5.

2. **Traefik Dashboard**

   Access the Traefik dashboard at `https://your-traefik-domain.com`. You will be prompted for the Traefik username and password configured in the `.env` file.

3. **Managing Services**

   - **Start Services:**

     ```bash
     docker-compose up -d
     ```

   - **Stop Services:**

     ```bash
     docker-compose down
     ```

   - **View Logs:**

     ```bash
     docker-compose logs -f
     ```

## Logging

The setup uses JSON-file logging with the following configuration:

- **Log Driver:** `json-file`
- **Max Size:** `1m`
- **Max File:** `1`

Logs are managed per service and can be viewed using Docker commands or integrated with external logging solutions like Promtail.

## Troubleshooting

- **Traefik Not Accessible:**

  - Ensure that ports `80` and `443` are open and not blocked by a firewall.
  - Verify that your domain DNS settings are correctly pointing to your server.

- **MT5 Not Starting:**

  - Check the logs of the `mt5` service for any installation errors.
  - Ensure that Wine dependencies are properly installed.

- **VNC Connection Issues:**
  - Confirm that the VNC service is running and accessible via the specified domain.
  - Verify network configurations and firewall settings.

## Contributing

Contributions are welcome! Please follow these steps:

1. **Fork the Repository**

2. **Create a Feature Branch**

   ```bash
   git checkout -b feature/YourFeature
   ```

3. **Commit Your Changes**

   ```bash
   git commit -m "Add Your Feature"
   ```

4. **Push to the Branch**

   ```bash
   git push origin feature/YourFeature
   ```

5. **Open a Pull Request**

## License

This project is licensed under the [MIT License](LICENSE.md).
# ai-bot-v4
