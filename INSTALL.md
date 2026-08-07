# YomieCRM Installation Guide

## Prerequisites

- Docker Engine 20.10+
- Docker Compose v2+
- Domain names pointing to your server:
  - `crm.yomie.be` (backend API)
  - `manager.yomie.be` (frontend)
  - `socket.yomie.be` (WebSocket)

## Installing Docker

### Debian / Ubuntu

```bash
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
sudo usermod -aG docker $USER
```

### RHEL / CentOS / AlmaLinux

```bash
sudo dnf install -y dnf-plugins-core
sudo dnf config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo
sudo dnf install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
sudo systemctl enable --now docker
sudo usermod -aG docker $USER
```

### Arch Linux

```bash
sudo pacman -S docker docker-compose
sudo systemctl enable --now docker
sudo usermod -aG docker $USER
```

### Windows

1. Download and install [Docker Desktop for Windows](https://docs.docker.com/desktop/install/windows-install/)
2. Enable WSL 2 backend during installation (recommended)
3. Open Docker Desktop and ensure it's running
4. Use PowerShell or Git Bash to run commands

> Note: On Windows, run `bash start.sh` from Git Bash or WSL.

### macOS

1. Download and install [Docker Desktop for Mac](https://docs.docker.com/desktop/install/mac-install/) (Apple Silicon or Intel)
2. Open Docker Desktop and ensure it's running
3. Or install via Homebrew:
```bash
brew install --cask docker
```

### Verify Installation

```bash
docker --version
docker compose version
```

Log out and back in after installation for group changes to take effect.

## Quick Start

```bash
# 1. Clone the repository
git clone https://github.com/lainard/YomieCRM7.4 YomieCRM
cd YomieCRM

# 2. Start all services (passwords auto-generated on first run)
./start.sh

# 3. Start with database import
./start.sh --import

# 4. Start without cron jobs (development)
./start.sh --disable-cron
```

## Step-by-Step Deployment

### 1. Configure Environment

Copy and edit the environment file:

```bash
cp .env.example .env
nano .env
```

Database passwords (`DB_ROOT_PASSWORD`, `DB_USER_PASSWORD`) are auto-generated on first run if left as `CHANGE_ME`.

Fill in your API credentials:
- `PUSHER_AUTH_KEY`, `PUSHER_SECRET`, `PUSHER_APP_ID`
- `SENDINBLUE_API_KEY`
- `EXACT_CLIENT_ID`, `EXACT_CLIENT_SECRET`
- `ZOHO_CLIENT_ID`, `ZOHO_CLIENT_SECRET`, `ZOHO_REFRESH_TOKEN`

### 2. SSL Certificates

Place your SSL certificates in the following structure:

```
docker/nginx/certs/
├── crm.yomie.be/
│   ├── fullchain.pem
│   └── privkey.pem
├── manager.yomie.be/
│   ├── fullchain.pem
│   └── privkey.pem
└── socket.yomie.be/
    ├── fullchain.pem
    └── privkey.pem
```

If certificates are not present, the system will attempt to obtain them via Let's Encrypt automatically (requires ports 80/443 accessible from the internet).

### 3. Database Import

Create a zip file at `docker/db/db.zip` containing the SQL dumps:

```
docker/db/db.zip
├── yomie_crm.sql    (main CRM database)
├── crm.sql          (WHMCS/billing database)
└── prod_crm.sql     (production/KBO database)
```

Each file MUST be named exactly as shown above. The filename (without `.sql`) determines which database it gets imported into.

To create the zip from existing dumps:
```bash
cd docker/db
zip db.zip yomie_crm.sql crm.sql prod_crm.sql
```

Then run:
```bash
./start.sh --import
```

This will:
1. Ask for confirmation (all existing data will be dropped)
2. Drop and recreate each database
3. Import the SQL files
4. Re-grant privileges to `yomie_user`

### 4. Start Services

```bash
./start.sh
```

## Architecture

```
Internet
    │
    ▼
┌─────────────────────────────────────────────────────┐
│  yomie_proxy (Nginx)  :80 / :443                    │
├─────────────────────────────────────────────────────┤
│  crm.yomie.be     → yomie_backend    (PHP 7.4)     │
│  manager.yomie.be → yomie_frontend   (Apache)      │
│  socket.yomie.be  → yomie_socket     (Node.js)     │
└─────────────────────────────────────────────────────┘
                         │
                         ▼
              ┌──────────────────────┐
              │  yomie_db (MariaDB)  │
              │  :3306               │
              └──────────────────────┘
```

## Containers

| Container | Image | Purpose | Port |
|-----------|-------|---------|------|
| yomie_proxy | Nginx 1.25 | Reverse proxy + SSL | 80, 443 |
| yomie_backend | PHP 7.4 + Apache | CI3 backend API | internal |
| yomie_frontend | PHP 7.4 + Apache | Angular SPA | internal |
| yomie_socket | Node.js 18 | Socket.IO server | internal |
| yomie_db | MariaDB 10.11 | Database | 3306 |

## File Structure

```
.
├── app/                          # Application source code
│   ├── application/              # CI3 application
│   ├── system/                   # CI3 system
│   ├── public/                   # Angular frontend (built)
│   ├── socket.io/                # Socket.IO server
│   ├── assets/                   # Static assets
│   ├── storage/                  # File storage
│   └── index.php                 # CI3 entry point
├── docker/
│   ├── app/                      # Backend Dockerfile + configs
│   ├── frontend/                 # Frontend Dockerfile + configs
│   ├── nginx/                    # Reverse proxy configs + certs
│   ├── db/                       # Database init + db.zip
│   └── README.md
├── docker-compose.yml
├── start.sh                      # Main start script
├── .env                          # Environment variables (secrets)
├── .env.example                  # Template
└── INSTALL.md                    # This file
```

## Persistent Data

The following data survives container restarts and rebuilds:

| Path | Type | Content |
|------|------|---------|
| `app/application/logs/` | Bind mount | PHP/CI3 logs |
| `app/application/cache/` | Bind mount | Application cache |
| `app/application/data/` | Bind mount | Application data |
| `app/logs/` | Bind mount | General logs |
| `app/storage/` | Bind mount | File uploads |
| `yomie_documents` | Docker volume | Documents |
| `yomie_db_data` | Docker volume | Database files |
| `letsencrypt_proxy` | Docker volume | SSL certificates |

## Common Commands

```bash
# View all logs
docker compose logs -f

# View specific container logs
docker compose logs -f yomie_backend

# Shell into backend
docker exec -it yomie_backend bash

# MySQL shell
docker exec -it yomie_db mysql -u root -p

# Run CI3 CLI command
docker exec -it yomie_backend php /var/www/html/index.php cron daily

# Restart a single service
docker compose restart yomie_backend

# Rebuild and restart
docker compose up -d --build yomie_backend

# Stop everything
docker compose down

# Stop and remove all data (DESTRUCTIVE)
docker compose down -v
```

## Troubleshooting

### CORS errors
CORS is handled by `yomie_proxy`. Check `docker/nginx/nginx.conf` for allowed headers.

### Database connection refused
Wait for `yomie_db` to show `(healthy)` in `docker ps`. The backend waits for this automatically.

### SSL certificate issues
- Ensure domains point to your server
- Check `docker/nginx/certs/` has proper cert files
- View proxy logs: `docker compose logs yomie_proxy`

### Cron not running
- Check if started with `--disable-cron`
- Verify inside container: `docker exec yomie_backend crontab -l`

### Permission errors on logs/storage
```bash
docker exec yomie_backend chown -R www-data:www-data /var/www/html/application/logs
docker exec yomie_backend chown -R www-data:www-data /var/www/html/storage
```
