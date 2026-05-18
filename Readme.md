# Private Infrastructure Stack

A modular self-hosted infrastructure stack using:

- Docker + Docker Compose
- Nginx Proxy Manager
- Portainer
- MariaDB
- Redis
- Poste.io
- Netdata
- Duplicati
- Uptime Kuma
- Dozzle
- Databasus

Designed for:
- Bare metal servers
- Small agency infrastructure
- Client app hosting
- Personal mail
- Centralized backups
- Low operational complexity

---

# Included Services

| Service | Purpose | Port |
|---|---|---|
| Nginx Proxy Manager | Reverse proxy + SSL | 81 |
| Portainer | Docker management | 9000 |
| MariaDB | Database server | 3306 |
| Redis | Cache/session store | 6379 |
| phpMyAdmin | Database management | 8081 |
| Uptime Kuma | Uptime monitoring | 3001 |
| Dozzle | Docker logs viewer | 9999 |
| Netdata | Hardware monitoring | 19999 |
| Duplicati | Backup management | 8200 |
| Poste.io | Mail server | 25/465/587 |
| Databasus | Database backups | 9260 |

---

# Requirements

- Ubuntu/Debian Server
- Docker
- Docker Compose
- Mounted backup storage (TrueNAS recommended)

---

# Install Docker

```bash
curl -fsSL https://get.docker.com | sh

systemctl enable docker
systemctl start docker
```

---

# Recommended Folder Structure

```bash
/opt/infra/
├── docker-compose.yml
├── .env
├── .env.example
└── data/
```

---

# Environment Variables

## Example `.env`

```env
TZ=Asia/Kolkata

HTTP_PORT=80
HTTPS_PORT=443

NPM_PORT=81
PORTAINER_PORT=9000
MARIADB_PORT=3306
REDIS_PORT=6379
PHPMYADMIN_PORT=8081
UPTIMEKUMA_PORT=3001
DOZZLE_PORT=9999
NETDATA_PORT=19999
DUPLICATI_PORT=8200
DATABASUS_PORT=9260

MAIL_SMTP_PORT=25
MAIL_POP3_PORT=110
MAIL_IMAP_PORT=143
MAIL_SMTPS_PORT=465
MAIL_SUBMISSION_PORT=587
MAIL_IMAPS_PORT=993
MAIL_POP3S_PORT=995

MYSQL_ROOT_PASSWORD=change-this
MYSQL_DATABASE=appdb
MYSQL_USER=appuser
MYSQL_PASSWORD=change-this-too

MARIADB_BUFFER_POOL=512M

POSTE_HOSTNAME=mail.example.com

SOURCE_PATH=/opt
BACKUP_PATH=/mnt/truenas
```

---

# GitHub Safety

Recommended `.gitignore`

```gitignore
.env
data/
```

Only commit:
- docker-compose.yml
- .env.example
- README.md

Never commit:
- real credentials
- production secrets
- backups
- docker volumes

---

# Quick Start

```bash
mkdir -p /opt/infra

cd /opt/infra

git clone YOUR_REPO .

cp .env.example .env

nano .env

docker compose up -d
```

---

# Service Access

| Service | URL |
|---|---|
| NPM | http://SERVER-IP:81 |
| Portainer | http://SERVER-IP:9000 |
| phpMyAdmin | http://SERVER-IP:8081 |
| Uptime Kuma | http://SERVER-IP:3001 |
| Dozzle | http://SERVER-IP:9999 |
| Netdata | http://SERVER-IP:19999 |
| Duplicati | http://SERVER-IP:8200 |
| Databasus | http://SERVER-IP:9260 |

---

# Backup Strategy

## Local
- Duplicati → TrueNAS

## Recommended Offsite Backup
- Backblaze B2
- Cloudflare R2
- Secondary VPS

---

# Suggested Bare Metal Layout

| Server | Purpose |
|---|---|
| Server 1 | Apps + Proxy |
| Server 2 | Databases + Monitoring |
| Server 3 | TrueNAS Backups |

---

# Philosophy

This stack intentionally avoids:
- Kubernetes complexity
- Heavy orchestration
- Overengineering
- Vendor lock-in
- Unnecessary abstraction

Goal:

```text
Simple
Modular
Maintainable
Reproducible
Reliable
```
