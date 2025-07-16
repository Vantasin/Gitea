# 📦 Gitea Docker Compose Stack

[![MIT License](https://img.shields.io/github/license/Vantasin/Gitea?style=flat-square)](LICENSE)
[![Docker Pulls: gitea/gitea](https://img.shields.io/docker/pulls/gitea/gitea?style=flat-square&logo=docker)](https://hub.docker.com/r/gitea/gitea)
[![ZFS](https://img.shields.io/badge/ZFS-OpenZFS-blue?style=flat-square)](https://openzfs.org/)

This repository contains a minimal and production-ready [Gitea](https://gitea.io/) stack using Docker Compose.  
**Gitea** is a lightweight, self-hosted Git service offering Git repository hosting, code review, issue tracking, and CI integration.

---

## 📁 Directory Structure

```bash
tank/
├── docker/
│   ├── compose/
│   │   └── gitea/              # Git repo lives here
│   │       ├── docker-compose.yml  # Main Docker Compose config
│   │       ├── .env                # Runtime environment variables and secrets (gitignored!)
│   │       ├── env.example         # Example .env file for reference
│   │       ├── env.template        # Optional template
│   │       └── README.md           # This file
│   └── data/
│       └── gitea/              # Volume mounts and persistent data
```

---

## 🧰 Prerequisites

* Docker Engine
* Docker Compose V2
* Git
* (Optional) ZFS on Linux for dataset management

> ⚠️ **Note:** These instructions assume your ZFS pool is named `tank`. If your pool has a different name (e.g., `rpool`, `zdata`, etc.), replace `tank` in all paths and commands with your actual pool name.

---

## ⚙️ Setup Instructions

1. **Create the stack directory and clone the repository**

   If using ZFS:
   ```bash
   sudo zfs create -p tank/docker/compose/gitea
   cd /tank/docker/compose/gitea
   sudo git clone https://github.com/Vantasin/Gitea.git .
   ```

   If using standard directories:
   ```bash
   mkdir -p ~/docker/compose/gitea
   cd ~/docker/compose/gitea
   git clone https://github.com/Vantasin/Gitea.git .
   ```

2. **Create the runtime data directory** (optional)

   If using ZFS:
   ```bash
   sudo zfs create -p tank/docker/data/gitea
   ```

   If using standard directories:
   ```bash
   mkdir -p ~/docker/data/gitea
   ```

3. **Configure environment variables**

   Copy and modify the `.env` file:

   ```bash
   sudo cp env.example .env
   sudo nano .env
   sudo chmod 600 .env
   ```

   > **Note:** Be sure to update the `GITEA_DB_PASSWORD`, `GITEA_ROOT_URL`, `GITEA_SSH_DOMAIN`, and if necessary the `GITEA_DATA_VOLUME`.

   > **Note:** Create the `GITEA_ROOT_URL` using [Nginx Proxy Manager](https://github.com/Vantasin/Nginx-Proxy-Manager.git) as a reverse proxy for HTTPS certificates via Let's Encrypt.

   <p align="center">
  <img
    src="images/proxy-host.png"
    alt="Gitea Proxy Host UI"
    style="width:50%; height:auto;"
  />
</p>

4. **Start Gitea**

   ```bash
   docker compose up -d
   ```

---

## 🌐 Accessing Gitea Web UI

Once deployed, access Gitea using:

- **Web Interface (HTTP):** `http://localhost:3001` or replace `localhost` with your server’s IP address.  
- **Initial Setup:** Follow the prompts to complete the initial setup.
  > **Note:** The field's are pre-populated based on the .env file you just created.
- **Add SSH Keys:** Go to Settings -> SSH/GPG Keys -> Manage SSH Keys -> Add Key
  > **Note:** SSH keys are typically found at `~/.ssh/id_rsa.pub`.

---

## 🙏 Acknowledgments

- [ChatGPT](https://openai.com/chatgpt) — for assistance in generating setup scripts and templates.
- [Gitea / docker-gitea](https://github.com/go-gitea/gitea) — the official source code and Docker image for Gitea.
- [Gitea Documentation](https://docs.gitea.io/) — the official user guide and admin reference for configuring and troubleshooting Gitea.
- [Docker](https://www.docker.com/) — for container orchestration and runtime.
- [OpenZFS](https://openzfs.org/) — for advanced local filesystem features, dataset organization, and snapshotting.
