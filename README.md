# Serveza 🖥️

**The zero-bullshit, self-hosted dashboard for your home server.**

**🔥 GET SERVEZA HERE:** [Download the Source Code & Setup files on Lemon Squeezy](https://placieslaf.lemonsqueezy.com/checkout/buy/45edebf2-c36f-4fca-a271-d9258715c3c4)

Are you tired of opening Portainer, Uptime Kuma, and a terminal just to check if your containers are running, your SSL certs are valid, and your backups didn't fail?

Serveza is a lightweight, single-page dashboard that brings the most critical health checks into one place. No databases. No cloud accounts. Configured entirely via a single YAML file.

---

## ✨ Features

- 📊 **System Health:** Live CPU, RAM, Disk usage, and Uptime (updates every 5s).
- 🐳 **Docker Management:** View all containers, their status, and restart them directly from the UI without touching the CLI.
- 🔒 **SSL Monitor:** Automatically checks expiration dates for your configured domains and warns you before they expire.
- 💾 **Backup Watchdog:** Monitors your backup directories. If no new files are created within X hours (e.g., your cronjob failed), it flags it in red.
- 🛡️ **Built-in Security:** Protected by HTTP Basic Auth out of the box.
- 🪶 **Stupidly Lightweight:** Built with FastAPI + HTMX. No heavy JS frameworks, no database to maintain.

---

## 🚀 Quick Start (Docker Compose)

Serveza is designed to be deployed in less than 2 minutes.

### 1. Extract the archive

Unzip the downloaded `serveza_v1.0.zip` file onto your server.

### 2. Configure your environment

Copy the example configuration file:

```bash
cp serveza.yaml.example serveza.yaml
```

Open `serveza.yaml` with your favorite editor and set up your preferences.
**Make sure to change the default username and password!**

### 3. Start the container

```bash
docker compose up -d --build
```

### 4. Access the dashboard

Open your browser and navigate to `http://<your-server-ip>:8123`.
Log in with the credentials you set in the YAML file.

---

## ⚙️ Configuration (`serveza.yaml`)

Serveza reads all its settings from `serveza.yaml`. Here is a full breakdown of every module.

### 🛡️ Authentication

```yaml
auth:
  enabled: true
  username: "admin"
  password: "ChangeThisNow!"
```

### 💽 Disk Monitoring

Add as many mount points as you want to monitor.

```yaml
monitoring:
  disk_paths:
    - "/"
    - "/mnt/storage"
  thresholds:
    cpu_percent: 85
    ram_percent: 90
    disk_percent: 90
```

### 🐳 Docker

```yaml
docker:
  enabled: true
  socket: "unix:///var/run/docker.sock"
```

### 🔒 SSL Certificates

List the domains you want to keep an eye on.

```yaml
ssl:
  enabled: true
  domains:
    - "my-nextcloud.local"
    - "example.com"
  warn_days_before: 30
```

### 💾 Backup Watchdog

Serveza checks if a directory has been updated recently. If the latest file is older than `max_age_hours`, the dashboard will show a red alert.

```yaml
backup:
  enabled: true
  paths:
    - "/backups"
  max_age_hours: 24
```

> **Important:** If you monitor a backup folder outside the Serveza directory, mount it as a read-only volume in `docker-compose.yml`:
>
> ```yaml
> volumes:
>   - /mnt/my_backups:/backups:ro
> ```

---

## 🐳 docker-compose.yml reference

```yaml
services:
  serveza:
    build: .
    container_name: serveza
    ports:
      - "8123:8123"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - ./serveza.yaml:/app/serveza.yaml:ro
      - /mnt/my_backups:/backups:ro  # optional
    restart: unless-stopped
```

---

## 🛠️ Tech Stack

- **Backend:** Python 3.12 + FastAPI
- **Frontend:** Jinja2 + HTMX (no React, no Vue, no overhead)
- **System data:** `psutil` + `docker-py`
- **Deploy:** Single Docker Compose stack

---

## 📝 Support & Updates

Thank you for purchasing Serveza! This is a one-time purchase — you get lifetime access to the code and can modify it freely for personal use.

If you encounter critical bugs or have feature requests, feel free to reach out via the email provided on your Gumroad receipt.
