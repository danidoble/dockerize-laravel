# Dockerize Laravel

This repository provides a seamless Docker setup for Laravel applications. It includes configuration for Nginx, PHP 8.4, PostgreSQL, Redis, and a workspace container with Node.js and other utilities.

## Installation

You can quickly add the Docker configuration to your existing Laravel project using our installation script.

### One-Line Install

Run the following command in your terminal from the root of your Laravel project:

```bash
curl -fsSL https://raw.githubusercontent.com/danidoble/dockerize-laravel/main/install | bash
```

> **Note:** Ensure you are in the root directory of your Laravel application before running the script.

### What the installer does:

1.  Downloads the Docker configuration files from this repository.
2.  Extracts and places them in your project root (`docker/`, `compose.dev.yaml`, `compose.prod.yaml`).
3.  Updates your `.gitignore` to include the new Docker files and data directories.
4.  Helps you configure your `.env` file by copying the provided example or identifying differences if one already exists.

## Usage

### Development

To start the development environment:

```bash
docker compose -f compose.dev.yaml up -d
```

This will spin up the following services:

- **Web**: Nginx (exposed on port `8000` by default).
- **PHP-FPM**: PHP 8.4 with Xdebug enabled.
- **Postgres**: PostgreSQL 18 database (exposed on port `5432`).
- **Redis**: Redis for caching and queues.
- **Workspace**: A container with PHP, Node.js, NPM, and other tools for running artisan commands, composer, etc.

You can access your application at `http://localhost:8000`.

### Running Commands

To run Artisan or Composer commands, use the workspace container:

```bash
# Enter the workspace container
docker compose -f compose.dev.yaml exec workspace bash

# Run commands
composer install
php artisan migrate
npm install
npm run dev
```

Alternatively, you can run commands directly without entering the shell:

```bash
docker compose -f compose.dev.yaml exec workspace php artisan migrate
```

### Production

For production environments, use the production compose file:

```bash
docker compose -f compose.prod.yaml up -d --build
```

Make sure to configure your production `.env` variables appropriately.

## Project Structure

- **docker/**: Contains Dockerfile definitions and configuration files for Nginx, PHP, etc.
  - **common/**: Shared configurations (e.g., PHP-FPM).
  - **development/**: Development-specific configs (e.g., Nginx dev config, Workspace).
  - **production/**: Production-specific configs.
- **compose.dev.yaml**: Docker Compose file for local development.
- **compose.prod.yaml**: Docker Compose file for production deployment.
- **.env.example**: Example environment variables optimized for this Docker setup.

## Customization

You can customize the ports and other settings in your `.env` file:

```dotenv
APP_URL="http://localhost:${NGINX_PORT:-8000}"

# Custom Ports
NGINX_PORT=8000
POSTGRES_PORT=5432
VITE_PORT=5173

# Xdebug
XDEBUG_ENABLED=true
```
