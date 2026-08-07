# YomieCRM 7.4

A full-featured CRM system built with CodeIgniter 3, Angular frontend, and Socket.IO real-time notifications.

## Prerequisites

- Docker Engine 20.10+
- Docker Compose v2+
- Git
- Minimum 2GB RAM, 10GB disk space
- Ports 80, 443, 3306 available
- Domain names pointing to your server:
  - `crm.yomie.be`
  - `manager.yomie.be`
  - `socket.yomie.be`
- SSL certificates in `docker/nginx/certs/` or Let's Encrypt auto-provisioning enabled
- Database dump file `docker/db/db.zip` containing `yomie_crm.sql`, `crm.sql`, `prod_crm.sql`

### Installing Docker

See [INSTALL.md](INSTALL.md) for Docker installation instructions on Debian, RHEL, Arch, Windows, and macOS.

After installing, log out and back in for group changes to take effect. Verify with:
```bash
docker --version
docker compose version
```

## Tech Stack

- **Backend**: PHP 7.4 + CodeIgniter 3 + Apache
- **Frontend**: Angular (pre-built SPA)
- **Database**: MariaDB 10.11
- **Real-time**: Socket.IO (Node.js)
- **Reverse Proxy**: Nginx (SSL termination + routing)
- **PDF Generation**: TCPDF
- **Containerization**: Docker Compose

## Services

| Service | Domain | Description |
|---------|--------|-------------|
| Backend API | crm.yomie.be | CodeIgniter 3 REST API |
| Frontend | manager.yomie.be | Angular management dashboard |
| WebSocket | socket.yomie.be | Real-time notifications and chat |
| API Docs | crm.yomie.be/swagger | Swagger/OpenAPI documentation |

## Integrations

- **Exact Online** - Accounting integration
- **Zoho CRM** - CRM sync
- **Mollie** - Payment processing
- **SendInBlue** - Email marketing
- **Pusher / Socket.IO** - Real-time events
- **Twikey** - Direct debit management
- **Peppol** - E-invoicing

## Quick Start

```bash
# Start all services
./start.sh

# Start with database import
./start.sh --import

# Start without cron jobs
./start.sh --disable-cron

# Combine flags
./start.sh --import --disable-cron
```

## Docker Containers

| Container | Image | Purpose |
|-----------|-------|---------|
| yomie_proxy | Nginx 1.25 | Reverse proxy, SSL, CORS |
| yomie_backend | PHP 7.4 + Apache | CI3 backend API |
| yomie_frontend | PHP 7.4 + Apache | Angular SPA |
| yomie_socket | Node.js 18 | Socket.IO server |
| yomie_db | MariaDB 10.11 | Database |

## Project Structure

```
.
├── app/                          # Application source
│   ├── application/              # CI3 MVC (controllers, models, views)
│   ├── system/                   # CI3 framework core
│   ├── assets/                   # Backend static assets
│   ├── public/                   # Angular frontend (built)
│   ├── socket.io/                # Socket.IO server
│   └── index.php                 # Entry point
├── docker/
│   ├── app/                      # Backend container config
│   ├── frontend/                 # Frontend container config
│   ├── nginx/                    # Reverse proxy + SSL certs
│   └── db/                       # Database init + import
├── docker-compose.yml
├── start.sh                      # Main start script
├── .env                          # Environment config (secrets)
├── .env.example                  # Template
├── INSTALL.md                    # Deployment guide
└── README.md                     # This file
```

## Environment Variables

All credentials are stored in `.env` (not committed to git). Database passwords are auto-generated on first run. See `.env.example` for the full list.

Key variables:
- `DB_ROOT_PASSWORD` / `DB_USER_PASSWORD` - Database credentials
- `PUSHER_*` - Real-time notification keys
- `SENDINBLUE_API_KEY` - Email service
- `EXACT_*` - Accounting integration
- `ZOHO_*` - CRM integration
- `CERTBOT_*` - SSL certificate management

## Database

Three MariaDB databases:
- `yomie_crm` - Main CRM data
- `crm` - Billing/WHMCS data
- `prod_crm` - Production/KBO data

Access via:
```bash
docker exec -it yomie_db mysql -u yomie_user -p
```

## Deployment

See [INSTALL.md](INSTALL.md) for full deployment instructions including SSL setup, database import, and production configuration.

## License

Copyright (c) 2024 Aditum IT. All rights reserved.
Author: Simson Lai <developer@aditum.be>

This software is proprietary and confidential. Unauthorized copying, distribution, or modification is strictly prohibited.
