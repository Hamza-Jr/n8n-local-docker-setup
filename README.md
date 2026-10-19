# Self-Hosted n8n with Docker Compose

A free, self-hosted workflow automation environment using Docker Compose, PostgreSQL, and ngrok. Run n8n locally on your own computer, store your data persistently, and manage all services from a single Compose configuration.

## Stack

- **n8n** — workflow automation platform.
- **PostgreSQL** — database for persistent n8n data.
- **Docker Compose** — manages the services, containers, and network.
- **ngrok** — provides a public HTTPS endpoint.

## Prerequisites

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) for Windows or macOS, or Docker Engine and the Docker Compose plugin on Linux.
- An [ngrok](https://ngrok.com/) account with an authentication token and configured HTTPS domain.

## Setup

### 1. Clone the repository

```bash
git clone https://github.com/Hamza-Jr/n8n-local-docker-setup.git
cd n8n-local-docker-setup
```

### 2. Configure environment variables

Copy the example environment file:

**Windows PowerShell**
```powershell
Copy-Item .env.example .env
```

**Linux / macOS**
```bash
cp .env.example .env
```

- Edit `.env` and configure your PostgreSQL credentials, n8n encryption key and version, and ngrok settings.
- Replace all placeholder values in `.env` with your own configuration before starting the services.


### 3. Prepare storage folders

Create two empty folders in the project directory:

- `n8n_data/`
- `postgres_data/`

**Windows PowerShell**
```powershell
New-Item -ItemType Directory -Force n8n_data, postgres_data
```

**Linux / macOS**
```bash
mkdir -p n8n_data postgres_data
```

These folders store n8n application data and PostgreSQL database files locally. Keep them empty before the first startup; Docker and the services will populate them as needed.


### 4. Start the services

```bash
docker compose up -d
```

### 5. Check the containers

```bash
docker compose ps
```

### 6. Open n8n

Visit [http://localhost:5678](http://localhost:5678) in your browser.

## Useful Commands

| Command | Description |
|---|---|
| `docker compose up -d` | Create and start services in the background. |
| `docker compose stop` | Stop services without removing containers. |
| `docker compose start` | Start previously stopped containers. |
| `docker compose restart` | Restart existing containers. |
| `docker compose down` | Stop and remove containers and the Compose network. |
| `docker compose ps` | Check container status. |
| `docker compose logs -f` | View live service logs. |

## Automatic Restart

Docker provides different restart policies:

- **`restart: always`** — Containers restart automatically after crashes or Docker restarts. They also restart when Docker starts again, even if they were manually stopped before the Docker restart.
- **`restart: unless-stopped`** — Containers restart automatically after crashes or Docker restarts, unless they were explicitly stopped manually. They remain stopped when Docker restarts.

**This project uses `restart: unless-stopped`** to give users more control over when their services run.

To start the services automatically when your computer boots, configure Docker Desktop to start automatically on Windows/macOS, or enable Docker at startup on Linux.


## Repository Structure

```text
n8n-local-docker-setup/
├── compose.yaml
├── .env.example
├── .gitignore
├── README.md
├── n8n_data/
└── postgres_data/
```

- **`compose.yaml`** — defines the services, network, ports, volumes, and environment variables.
- **`.env.example`** — provides example environment variables for configuration.
- **`.gitignore`** — excludes secrets, local data, and other files that should not be committed.
- **`README.md`** — documents installation, configuration, and usage.
- **`n8n_data/`** — stores n8n application data and settings.
- **`postgres_data/`** — stores PostgreSQL database files.

The data directories are created locally when needed. They should not be committed to GitHub.

## Security

- Never commit your `.env` file, authentication tokens, or real passwords.
- Keep your n8n encryption key unchanged when using existing data.
- Back up your data directories before making significant changes.

