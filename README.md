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

  This repository provides a complete, reproducible template for running Seafile MC 
  with both SeaDoc and ONLYOFFICE in a modern homelab or small‑business environment.

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

<img width="1024" height="899" alt="image" src="https://github.com/user-attachments/assets/5cde8e0d-48a6-4b45-9cbb-09669e9ed190" />

---
🛠 Environment Variables
Create a .env file based on the included .env

🛠 Docker Compose yaml
Create a docker-compose.yaml based on the included .yaml

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

<img width="617" height="106" alt="Screenshot 2026-06-25 at 11 14 55 AM" src="https://github.com/user-attachments/assets/85aa7ea8-cbbd-468c-b003-247719e6dc15" />

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

https://manual.seafile.com/latest/
