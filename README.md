<img width="593" height="521" alt="Screenshot 2026-06-25 at 11 08 47 AM" src="https://github.com/user-attachments/assets/125acba1-5f80-4b36-8c8a-d7e6bd9fb37f" />
Seafile MC 13.x + SeaDoc + ONLYOFFICE + Caddy — Docker Deployment
A fully working, production‑ready Docker Compose deployment of Seafile MC 13.x, including:
SeaDoc Server (real‑time collaborative editing)
ONLYOFFICE Document Server
MariaDB 10.11
Redis
Memcached
Caddy reverse proxy
Clean network separation
JWT‑secured document editing
This repository provides a complete, reproducible template for running Seafile MC with both SeaDoc and ONLYOFFICE in a modern homelab or small‑business environment.
---
📌 Features
✔ SeaDoc real‑time collaboration
Supports .sdoc documents with live multi‑user editing.
✔ ONLYOFFICE integration
Full editing for .docx, .xlsx, .pptx.
✔ Caddy reverse proxy
Handles HTTPS termination and routing for all services.
✔ Clean network separation
Two networks:
seafile-net → internal services
caddy-net → public‑facing services
✔ Production‑ready
Persistent volumes, health checks (optional), and clean service dependencies.
---
📦 Components
Service	Purpose
MariaDB 10.11	Seafile database backend
Redis	Pub/Sub for SeaDoc + Seahub
Memcached	Seahub caching
Seafile MC 13.x	Main application server
SeaDoc Server	Real‑time collaborative document editing
ONLYOFFICE Document Server	Office document editing
Caddy	Reverse proxy + HTTPS
---
🧩 Architecture Diagram
<img width="593" height="521" alt="Screenshot 2026-06-25 at 11 08 47 AM" src="https://github.com/user-attachments/assets/af72f444-1145-45d4-8826-0f6d5eb022fc" />


                   ┌──────────────────────────────┐
                   │            Clients            │
                   └───────────────┬──────────────┘
                                   │
                          HTTPS / HTTP
                                   │
                        ┌──────────▼──────────┐
                        │        Caddy        │
                        └───────┬─────┬──────┘
                                │     │
                     ┌──────────┘     └──────────┐
                     ▼                             ▼
            ┌────────────────┐            ┌──────────────────┐
            │    Seafile     │            │   ONLYOFFICE      │
            │  (MC 13.x)     │            │ Document Server   │
            └──────┬─────────┘            └──────────────────┘
                   │
                   ▼
         ┌──────────────────────┐
         │      SeaDoc          │
         │  (Standalone 2.0.9)  │
         └─────────┬────────────┘
                   │
     ┌─────────────▼──────────────┐
     │   MariaDB / Redis / Memcached │
     └──────────────────────────────┘

---
📁 Directory Layout
.
├── docker-compose.yml
├── Caddyfile
├── db/
├── seafile-data/
├── seadoc-data/
├── onlyoffice/
│   ├── data/
│   └── logs/
└── caddy/
    ├── data/
    └── config/

All paths are relative and safe for public sharing.
---
🛠 Environment Variables
Create a .env file based on this template:
SEAFILE_IMAGE=seafileltd/seafile-mc:13.0.24
SEAFILE_DB_IMAGE=mariadb:10.11
SEAFILE_REDIS_IMAGE=redis:7-alpine

SEAFILE_MYSQL_DB_HOST=db
SEAFILE_MYSQL_DB_PORT=3306
SEAFILE_MYSQL_DB_USER=seafile
SEAFILE_MYSQL_DB_PASSWORD=CHANGE_ME

INIT_SEAFILE_MYSQL_ROOT_PASSWORD=CHANGE_ME

SEAFILE_MYSQL_DB_CCNET_DB_NAME=ccnet_db
SEAFILE_MYSQL_DB_SEAFILE_DB_NAME=seafile_db
SEAFILE_MYSQL_DB_SEAHUB_DB_NAME=seahub_db

TIME_ZONE=UTC
SEAFILE_SERVER_HOSTNAME=example.com
SEAFILE_SERVER_PROTOCOL=https

ENABLE_SEADOC=true
SEADOC_SERVER_URL=http://seadoc:7070

JWT_PRIVATE_KEY=CHANGE_ME
REDIS_PASSWORD=CHANGE_ME

---
🚀 Deployment
Start the full stack:
docker compose up -d

Check logs:
docker compose logs -f

Stop the stack:
docker compose down

---
🔧 Caddy Reverse Proxy
Example Caddyfile:
example.com {
    reverse_proxy / http://seafile:80
    reverse_proxy /sdoc/* http://seadoc:7070
    reverse_proxy /onlyoffice/* http://onlyoffice-documentserver:80
}

---
🧪 Testing
SeaDoc
Open an .sdoc file → SeaDoc editor should load.
ONLYOFFICE
Open a .docx → ONLYOFFICE editor should load.
Seafile
Login → upload → sync → share → all should work normally.
---
🔐 Security Notes
Replace all CHANGE_ME values in .env
Use HTTPS (Caddy handles this automatically)
Use strong DB + JWT secrets
Restrict external DB access
Keep images updated
---
📦 Backup Strategy
Backup:
./seafile-data
./db
./seadoc-data
./onlyoffice/data
Plus a SQL dump:
mysqldump -u root -p --all-databases > seafile-backup.sql

---
📚 Credits
This deployment is based on real‑world testing and refinement in a homelab environment.
It is intended as a clean, reproducible template for anyone deploying Seafile MC with SeaDoc and ONLYOFFICE.

