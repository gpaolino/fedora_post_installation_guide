# 🐧 Fedora Workstation / KDE Plasma Post-Installation Guide

This guide contains the main setup commands, tweaks, and comments for configuring **Fedora Linux Workstation** (GNOME or KDE Plasma) after installation.

---

## 🧭 0. Before You Start

### 🔒 Backup Your Data

Make sure to back up your personal data from your previous installation, this is an example run on my environment:

```bash
rsync -avz --delete /home/paolo/ /run/media/paolo/'My Book'/OLD_BACKUPS/BackupThinkPad_20251019/
```

📘 **Beginner Resources:**
- 🎥 [15 Essential Tweaks for New Fedora Workstation Users (YouTube)](https://www.youtube.com/watch?v=GoCPO_If7kY)
- 🧾 [Paul Sorensen’s Fedora KDE Post-Installation Guide](https://paulsorensen.io/fedora-kde-plasma-post-installation-guide/)

---

## ⚙️ 0.1 System Setup

### 🔄 Update the System
```bash
sudo dnf check-update
```

### 💻 Set the Hostname
```bash
sudo hostnamectl set-hostname <new-hostname>
```

If you’ve already launched Chromium-based browsers (like Vivaldi), delete the lock file:
```bash
sudo rm ~/.config/vivaldi/SingletonLock
```

### 🧹 Remove Unwanted Preinstalled Apps (Bloatware)
```bash
sudo dnf remove libreoffice\* dragon neochat kpat kmines
```

---

## 🧩 1. GNOME Tweaks (Not for KDE)

- **Install:** Gnome Tweaks → via Software Store  
- **Dash to Dock Extension:**  
  👉 [GNOME Extensions Page](https://extensions.gnome.org/extension/307/dash-to-dock/)

---

## 💿 2. Enable RPM Fusion Repositories

RPM Fusion provides extra software not included in Fedora by default (e.g. proprietary drivers, codecs, and closed-source packages).

- **Free**: Open-source software Fedora excludes for policy reasons  
- **Non-Free**: Proprietary software (e.g. NVIDIA drivers, codecs)

```bash
sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
sudo dnf install https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

📦 **Multimedia Support:**
```bash
sudo dnf install ffmpeg
sudo dnf swap ffmpeg-free ffmpeg --allowerasing
sudo dnf install vlc
```

🎵 **Spotify:**
```bash
sudo dnf install spotify
```

---

## 📦 3. Flatpak Configuration

Fedora includes its own Flatpak remote, but it’s recommended to switch to **Flathub**, the main Flatpak repository:

```bash
flatpak remote-delete fedora
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

---

## 🧰 4. Btrfs Assistant + btrbk

**Btrfs Assistant** provides a GUI for creating and managing snapshots.  
**btrbk** handles automated incremental backups (local or remote).

```bash
sudo dnf install btrfs-assistant btrbk
sudo snapper -c root create-config /
sudo snapper -c home create-config /home
sudo snapper list-configs
sudo snapper -c root set-config ALLOW_USERS=$USER SYNC_ACL=yes
sudo snapper -c home set-config ALLOW_USERS=$USER SYNC_ACL=yes
```

🕐 Set snapshot frequency in **Btrfs Assistant → Snapper**.

📺 Learn more:
- [Install Fedora with Snapshot & Rollback Support (YouTube)](https://www.youtube.com/watch?v=J215Ctr90EI&t=260s)
- [Sysguides: Fedora 41 with Btrfs Snapshots](https://sysguides.com/install-fedora-41-with-snapshot-and-rollback-support)

---

## 🔐 5. Enable SSH (Optional)

```bash
sudo dnf install openssh-server
sudo systemctl enable --now sshd
sudo systemctl status sshd
sudo firewall-cmd --permanent --zone=public --add-service=ssh
sudo firewall-cmd --reload
```

---

## 🖥️ 6. Enable XRDP (Optional)

🔗 [Video Guide](https://www.youtube.com/watch?v=bPU2SGiiZck)

```bash
sudo dnf -y install xrdp tigervnc-server
sudo systemctl enable --now xrdp
sudo systemctl status xrdp
reboot
```

---

## ⚡ 7. Fastfetch

A modern system information tool (like neofetch but faster).

```bash
sudo dnf install fastfetch
```

---

## 🧩 8. Applications

### 📝 OnlyOffice
```bash
sudo dnf install https://download.onlyoffice.com/install/desktop/editors/linux/onlyoffice-desktopeditors.x86_64.rpm
```

### 💻 Sublime Text
```bash
sudo rpm -v --import https://download.sublimetext.com/sublimehq-rpm-pub.gpg
sudo dnf config-manager addrepo --from-repofile=https://download.sublimetext.com/rpm/stable/x86_64/sublime-text.repo
sudo dnf install sublime-text
```

### 🧠 Visual Studio Code
```bash
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\nautorefresh=1\ntype=rpm-md\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" | sudo tee /etc/yum.repos.d/vscode.repo > /dev/null
sudo dnf check-update
sudo dnf install code
```

👉 Enable **Native Title Bar** in VS Code:
> `CTRL+P` → `settings.json` → add `"window.titleBarStyle": "native"` → Restart VS Code.

---

## 🧰 9. Additional Software

| App | Command |
|-----|----------|
| **FileZilla** | `sudo dnf install filezilla` |
| **LocalSend** | `sudo snap install localsend` |
| **Syncthing** | `sudo dnf install syncthing` |
| **Google Chrome** | `sudo dnf install fedora-workstation-repositories && sudo dnf config-manager setopt google-chrome.enabled=1 && sudo dnf install google-chrome-stable` |
| **Brave Browser** | `sudo dnf install dnf-plugins-core && sudo dnf config-manager addrepo --from-repofile=https://brave-browser-rpm-release.s3.brave.com/brave-browser.repo && sudo rpm --import https://brave-browser-rpm-release.s3.brave.com/brave-core.asc && sudo dnf install brave-browser` |
| **yt-dlp (Snap)** | `sudo dnf install snap && sudo snap install --edge yt-dlp && sudo snap refresh --edge yt-dlp` |
| **Git** | `sudo dnf install git` |

---

## 🧠 Tip
> After setting everything up, reboot your system to apply all configuration changes.

---

**Happy Hacking! 🦾**  
Made with ❤️ for Fedora users by [gipa]
