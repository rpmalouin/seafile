Seafile MC 13.x + SeaDoc + ONLYOFFICE + Caddy (Full Docker Stack)
A fully working, production‑ready Docker Compose deployment of Seafile MC 13.x, including:
SeaDoc (standalone backend)
ONLYOFFICE Document Server
MariaDB 10.11
Redis
Memcached
Caddy reverse proxy
Clean network separation
JWT‑secured document editing
Full compatibility with Seafile 13.x
This stack is designed for homelab, small business, and self‑hosted cloud environments.
---
📌 Overview
This deployment uses a multi‑container architecture with two isolated networks:
seafile-net → internal services (DB, Redis, Memcached, Seafile, SeaDoc)
caddy-net → public‑facing services (Caddy, Seafile, SeaDoc, ONLYOFFICE)
This prevents port conflicts, isolates internal traffic, and ensures SeaDoc and ONLYOFFICE integrate cleanly with Seafile MC.
---
📦 Components
Service	Purpose
MariaDB 10.11	Seafile database backend
Redis	Pub/Sub for SeaDoc + Seahub
Memcached	Seahub caching
Seafile MC 13.x	Main application server
SeaDoc Server	Real‑time collaborative document editing
ONLYOFFICE Document Server	Office document editing (.docx, .xlsx, .pptx)
Caddy	Reverse proxy + HTTPS termination
---
🧩 Architecture Diagram
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
🚀 Features
✔ SeaDoc real‑time collaboration
Supports .sdoc documents with live editing.
✔ ONLYOFFICE integration
Full editing for .docx, .xlsx, .pptx.
✔ JWT‑secured document editing
Shared JWT key between Seafile, SeaDoc, and ONLYOFFICE.
✔ Clean network separation
Avoids port collisions and internal Nginx conflicts.
✔ Production‑ready
Persistent volumes, health checks (optional), and clean service dependencies.
---
📁 Directory Layout
/appdata/seafile/
    ├── data/           # Seafile shared data
    ├── db/             # MariaDB data
    ├── caddy/          # Caddy config + data
    ├── seadoc-data/    # SeaDoc storage
/appdata/onlyoffice/
    ├── data/
    ├── logs/

---
🛠 Environment Variables
Your .env file should define:
SEAFILE_IMAGE=seafileltd/seafile-mc:13.0.24
SEAFILE_DB_IMAGE=mariadb:10.11
SEAFILE_REDIS_IMAGE=redis:7-alpine

SEAFILE_MYSQL_DB_HOST=db
SEAFILE_MYSQL_DB_PORT=3306
SEAFILE_MYSQL_DB_USER=seafile
SEAFILE_MYSQL_DB_PASSWORD=yourpassword

INIT_SEAFILE_MYSQL_ROOT_PASSWORD=yourrootpass

SEAFILE_MYSQL_DB_CCNET_DB_NAME=ccnet_db
SEAFILE_MYSQL_DB_SEAFILE_DB_NAME=seafile_db
SEAFILE_MYSQL_DB_SEAHUB_DB_NAME=seahub_db

TIME_ZONE=America/New_York
SEAFILE_SERVER_HOSTNAME=your.domain.com
SEAFILE_SERVER_PROTOCOL=https

ENABLE_SEADOC=true
SEADOC_SERVER_URL=http://seadoc:7070

JWT_PRIVATE_KEY=YourSuperSecretKey

---
📜 Docker Compose (Working Version)
Note: This README does not include the YAML inline to avoid accidental edits.
Your working YAML should be committed as docker-compose.yml in this repo.
---
🔧 Caddy Reverse Proxy
Your Caddyfile should route:
/ → Seafile (8085)
/seafhttp → Seafile file server
/sdoc/ → SeaDoc (7070)
/onlyoffice/ → ONLYOFFICE
Example:
your.domain.com {
    reverse_proxy / http://seafile:80
    reverse_proxy /sdoc/* http://seadoc:7070
    reverse_proxy /onlyoffice/* http://onlyoffice-documentserver:80
}

---
🧪 Testing the Deployment
SeaDoc
Open an .sdoc file → should load the SeaDoc editor.
ONLYOFFICE
Open a .docx → should load the OO editor.
Seafile
Login → upload → sync → share → all should work.
---
🔐 Security Notes
Use HTTPS (Caddy handles this automatically).
Rotate JWT keys periodically.
Use strong DB passwords.
Restrict external DB access.
Keep images updated.
---
📦 Backup Strategy
Backup:
/appdata/seafile/data
/appdata/seafile/db
/appdata/seafile/seadoc-data
/appdata/onlyoffice/data
Plus a SQL dump:
mysqldump -u root -p --all-databases > seafile-backup.sql

---
📚 Credits
This deployment was assembled through extensive testing, debugging, and refinement in a real homelab environment.
Special thanks to the Seafile community for documentation and reference implementations.
