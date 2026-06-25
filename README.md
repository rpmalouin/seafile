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
