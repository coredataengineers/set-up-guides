# Docker Installation Guide
### Complete Setup Instructions for Windows, macOS, and Linux

---

## Table of Contents

1. [Overview](#1-overview)
2. [Windows](#2-windows)
3. [macOS](#3-macos)
4. [Linux](#4-linux)
   - [Ubuntu / Debian](#41-ubuntu--debian)
   - [Fedora / RHEL / CentOS](#42-fedora--rhel--centos)
   - [Arch Linux](#43-arch-linux)
5. [Verify Your Installation](#5-verify-your-installation)
6. [Post-Installation Steps](#6-post-installation-steps)
7. [Uninstalling Docker](#7-uninstalling-docker)
8. [Troubleshooting](#8-troubleshooting)

---

## 1. Overview

Docker is available in two main forms:

| Package | Description | Best For |
|---|---|---|
| **Docker Desktop** | GUI app bundling the Docker engine, Compose, and tooling | Windows & macOS |
| **Docker Engine** | Headless CLI-only daemon | Linux servers & power users |

Docker Compose is bundled with Docker Desktop. On Linux, it is installed separately as the `docker-compose-plugin`.

---

## 2. Windows

### System Requirements

| Requirement | Details |
|---|---|
| OS | Windows 10 (64-bit, Build 19041+) or Windows 11 |
| RAM | 4 GB minimum (8 GB recommended) |
| CPU | 64-bit processor with hardware virtualisation enabled in BIOS |
| Backend | WSL 2 (recommended) or Hyper-V |

### Step 1 — Enable WSL 2

Open **PowerShell as Administrator** and run:

```powershell
wsl --install
```

This installs WSL 2 and Ubuntu by default. Restart your machine when prompted.

Verify WSL 2 is active:

```powershell
wsl --list --verbose
```

You should see your distro listed with `VERSION 2`.

### Step 2 — Enable hardware virtualisation (if not already on)

Restart your machine, enter BIOS/UEFI settings, and enable:
- **Intel:** Intel VT-x
- **AMD:** AMD-V / SVM

This setting is usually found under **Advanced → CPU Configuration**.

### Step 3 — Download Docker Desktop

Go to **https://www.docker.com/products/docker-desktop** and click **Download for Windows**.

### Step 4 — Install Docker Desktop

1. Run the downloaded `Docker Desktop Installer.exe`.
2. On the configuration screen, ensure **Use WSL 2 instead of Hyper-V** is checked.
3. Click **OK** and follow the prompts.
4. Restart your machine when the installer finishes.

### Step 5 — Configure WSL 2 integration

1. Open Docker Desktop.
2. Go to **Settings → Resources → WSL Integration**.
3. Enable integration for your Linux distro (e.g. Ubuntu).
4. Click **Apply & Restart**.

### Step 6 — Verify

Open PowerShell or Windows Terminal and run:

```powershell
docker --version
docker compose version
```

> **Tip:** Docker Desktop must be running (visible in the system tray) before any `docker` command will work.

---

## 3. macOS

### System Requirements

| Requirement | Intel Mac | Apple Silicon (M1/M2/M3) |
|---|---|---|
| OS | macOS 12 Monterey or later | macOS 12 Monterey or later |
| RAM | 4 GB minimum | 4 GB minimum |
| Architecture | x86_64 | arm64 |

### Step 1 — Download Docker Desktop

Go to **https://www.docker.com/products/docker-desktop** and select the correct build:

- **Apple Silicon (M1/M2/M3):** Download for Mac with Apple chip
- **Intel Mac:** Download for Mac with Intel chip

Not sure which you have? Click the **Apple menu → About This Mac** and check the chip field.

### Step 2 — Install Docker Desktop

1. Open the downloaded `.dmg` file.
2. Drag **Docker.app** into your **Applications** folder.
3. Open Docker from Applications or Spotlight.
4. Approve the system extension prompt when asked.
5. Docker will start — look for the whale icon in your menu bar.

### Step 3 — (Apple Silicon only) Enable Rosetta

For best compatibility with x86 images on Apple Silicon:

1. Go to **Settings → General**.
2. Enable **Use Rosetta for x86_64/amd64 emulation on Apple Silicon**.
3. Click **Apply & Restart**.

### Step 4 — Verify

Open **Terminal** and run:

```bash
docker --version
docker compose version
```

### Installing via Homebrew (alternative)

If you use Homebrew, you can install Docker Desktop with:

```bash
brew install --cask docker
```

Then launch Docker from Applications as normal.

---

## 4. Linux

On Linux, you install **Docker Engine** (the daemon) plus the **Compose plugin** directly — no Desktop GUI is required, though Docker Desktop for Linux is available if you want it.

---

### 4.1 Ubuntu / Debian

Tested on: Ubuntu 20.04, 22.04, 24.04 and Debian 11, 12.

#### Remove old versions

```bash
sudo apt remove docker docker-engine docker.io containerd runc
```

#### Install dependencies

```bash
sudo apt update
sudo apt install -y ca-certificates curl gnupg lsb-release
```

#### Add Docker's official GPG key

```bash
sudo install -m 0755 -d /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
  sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

> For **Debian**, replace `ubuntu` with `debian` in the URL above.

#### Add the Docker repository

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

#### Install Docker Engine and Compose plugin

```bash
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

#### Start and enable the Docker service

```bash
sudo systemctl enable --now docker
```

---

### 4.2 Fedora / RHEL / CentOS

Tested on: Fedora 38+, RHEL 8/9, CentOS Stream 8/9.

#### Remove old versions

```bash
sudo dnf remove docker docker-client docker-client-latest docker-common \
  docker-latest docker-latest-logrotate docker-logrotate docker-engine
```

#### Add Docker's repository

```bash
sudo dnf install -y dnf-plugins-core

sudo dnf config-manager --add-repo \
  https://download.docker.com/linux/fedora/docker-ce.repo
```

> For **RHEL / CentOS**, replace `fedora` with `rhel` or `centos` in the repo URL.

#### Install Docker Engine and Compose plugin

```bash
sudo dnf install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

#### Start and enable the Docker service

```bash
sudo systemctl enable --now docker
```

---

### 4.3 Arch Linux

#### Install Docker

```bash
sudo pacman -S docker docker-compose
```

#### Start and enable the Docker service

```bash
sudo systemctl enable --now docker
```

---

## 5. Verify Your Installation

Run the following commands on any platform to confirm Docker is working correctly.

### Check versions

```bash
docker --version
docker compose version
```

### Run the hello-world container

```bash
docker run hello-world
```

Expected output includes:

```
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

### Check the Docker daemon status (Linux)

```bash
sudo systemctl status docker
```

---

## 6. Post-Installation Steps

### Run Docker without sudo (Linux only)

By default, Docker commands require `sudo` on Linux. To run as a non-root user:

```bash
sudo groupadd docker          # create the docker group (may already exist)
sudo usermod -aG docker $USER # add your user to the group
newgrp docker                 # apply the change in the current session
```

Log out and back in for the change to take full effect, then verify:

```bash
docker run hello-world        # should work without sudo
```

> **Security note:** Users in the `docker` group have effective root-equivalent privileges. Only add trusted users.

### Configure Docker to start on boot (Linux)

```bash
sudo systemctl enable docker
sudo systemctl enable containerd
```

### Set resource limits (Docker Desktop — Windows & macOS)

By default, Docker Desktop uses up to 50% of host RAM. To adjust:

1. Open Docker Desktop → **Settings → Resources**.
2. Set **Memory**, **CPU**, and **Disk** limits to match your workload.
3. Click **Apply & Restart**.

For the Spark stack in this project, allocating at least **4 GB RAM** and **2 CPUs** is recommended.

---

## 7. Uninstalling Docker

### Windows

1. Open **Settings → Apps → Installed Apps**.
2. Search for **Docker Desktop** and click **Uninstall**.
3. Optionally delete leftover data:

```powershell
Remove-Item -Recurse -Force "$env:APPDATA\Docker"
Remove-Item -Recurse -Force "$env:LOCALAPPDATA\Docker"
```

### macOS

1. Open Docker Desktop and go to the bug icon (top right) → **Uninstall**.
2. Or drag **Docker.app** from Applications to Trash.
3. Remove leftover files:

```bash
rm -rf ~/Library/Group\ Containers/group.com.docker
rm -rf ~/Library/Containers/com.docker.docker
rm -rf ~/.docker
```

### Ubuntu / Debian (Linux)

```bash
sudo apt purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
sudo rm /etc/apt/sources.list.d/docker.list
sudo rm /etc/apt/keyrings/docker.gpg
```

### Fedora / RHEL / CentOS (Linux)

```bash
sudo dnf remove docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```

---

## 8. Troubleshooting

### `docker: command not found` after installation

- **Windows/macOS:** Docker Desktop may not be running. Check the system tray/menu bar for the whale icon and start it.
- **Linux:** The Docker service may not be running. Run `sudo systemctl start docker`.

### `permission denied` when running docker (Linux)

You are not in the `docker` group. Run:

```bash
sudo usermod -aG docker $USER
newgrp docker
```

### Docker Desktop fails to start on Windows

- Confirm WSL 2 is installed: `wsl --list --verbose` in PowerShell.
- Ensure hardware virtualisation is enabled in BIOS.
- Try reinstalling WSL: `wsl --update`.

### `Cannot connect to the Docker daemon` (Linux)

The daemon is not running. Start it:

```bash
sudo systemctl start docker
```

Then check its status:

```bash
sudo systemctl status docker
```

### Slow performance on macOS with Apple Silicon

- Enable **Rosetta** under Settings → General for better x86 image compatibility.
- Enable **VirtioFS** under Settings → General → Virtual file sharing for faster volume mounts.

### Port conflicts on startup

If a container fails because a port is already in use:

```bash
# macOS / Linux
lsof -i :<port>

# Windows
netstat -ano | findstr <port>
```

Stop the conflicting process or change the host-side port mapping in your `docker-compose.yaml`.

---

*Docker Installation Guide — Windows · macOS · Linux*
