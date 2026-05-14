<p align="center">
  <img src="icon/banner.png" alt="Marix - A Modern, Zero-Knowledge SSH Client" width="100%">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/platform-Windows%20%7C%20macOS%20%7C%20Linux-blue" alt="Platform">
  <a href="https://github.com/marixdev/marix/actions/workflows/test.yml">
    <img src="https://github.com/marixdev/marix/actions/workflows/test.yml/badge.svg?branch=main" alt="CI Status">
  </a>
  <img src="https://img.shields.io/badge/license-GPL--3.0-blue" alt="License">
  <img src="https://img.shields.io/badge/client--side%20encryption-🔒-critical" alt="Client-Side Encryption">
 <a href="https://github.com/marixdev/marix/releases/latest">
  <img src="https://img.shields.io/github/v/release/marixdev/marix?color=orange&label=version" alt="Latest Version">
</a>
</p>


<p align="center">
  <a href="https://marix.dev">🌐 Website</a> •
  <a href="https://discord.gg/KSenHkCtN6">💬 Discord</a> •
  <a href="#-download">Download</a> •
  <a href="#-features">Features</a> •
  <a href="#-security">Security</a> •
  <a href="#-languages">Languages</a>
</p>

---

## 🌍 Other Languages

| | | | |
|---|---|---|---|
| 🇻🇳 [Tiếng Việt](lang/README.vi.md) | 🇮🇩 [Bahasa Indonesia](lang/README.id.md) | 🇨🇳 [中文](lang/README.zh.md) | 🇰🇷 [한국어](lang/README.ko.md) |
| 🇯🇵 [日本語](lang/README.ja.md) | 🇫🇷 [Français](lang/README.fr.md) | 🇩🇪 [Deutsch](lang/README.de.md) | 🇪🇸 [Español](lang/README.es.md) |
| 🇹🇭 [ภาษาไทย](lang/README.th.md) | 🇲🇾 [Bahasa Melayu](lang/README.ms.md) | 🇷🇺 [Русский](lang/README.ru.md) | 🇵🇭 [Filipino](lang/README.fil.md) |
| 🇧🇷 [Português](lang/README.pt.md) | | | |

---

## 🎯 Who is Marix for?

- **Developers & DevOps engineers** managing multiple servers
- **System administrators** who value security and efficiency
- **Security-conscious users** who don't trust cloud-based solutions
- **Anyone** who wants full control over their SSH credentials

---

## ⚠️ Disclaimer

> **YOU ARE RESPONSIBLE FOR YOUR OWN DATA.**
>
> Marix stores all data locally on your device with strong encryption. However:
> - **We cannot recover your data** if you lose your backup password
> - **We have no servers** - there is no "forgot password" option
> - **Backup regularly** - hardware can fail
> - **You own your security** - we provide the tools, you make the decisions
>
> By using Marix, you accept full responsibility for your data security.

---

## 🔒 Client-Side Encryption Architecture

> **"Your keys. Your servers. Your privacy."**

### Threat Model

Marix is designed for the following security assumptions:

> ⚠️ **Marix assumes a local, non-compromised host environment.**  
> It does not attempt to defend against malicious OS-level adversaries or compromised runtimes.

**In scope (protected against):**
- Theft of backup files without password
- Brute-force password attacks on encrypted backups
- Data tampering in transit or storage (detected via AEAD)
- Cloud provider access to your data (client-side encryption)

**Out of scope (not protected against):**
- Malware with root/admin access on your device
- Physical access to unlocked device with app running
- Keyloggers or screen capture malware
- Compromised operating system or Electron runtime

### What Marix Does NOT Do

| ❌ | Description |
|----|-------------|
| **No remote key storage** | Private keys never leave your device |
| **No key escrow** | We cannot recover your keys under any circumstance |
| **No recovery without password** | Lost password = lost backup (by design) |
| **No network calls during encryption** | Crypto operations are 100% offline |
| **No cloud servers** | We don't operate any infrastructure |
| **No telemetry** | Zero analytics, zero tracking, zero data collection |

### Core Principles

| | Principle | Description |
|---|-----------|-------------|
| 🔐 | **100% Offline** | All credentials stored locally on your device—never uploaded |
| ☁️ | **No Cloud** | We don't have servers. Your data never touches the internet |
| 📊 | **No Telemetry** | Zero tracking, zero analytics, zero data collection |
| 🔓 | **Open Source** | Fully auditable code under GPL-3.0, no hidden backdoors |

### Encryption Technology

| | Feature | Technology | Description |
|---|---------|------------|-------------|
| 🛡️ | **Local Storage** | Argon2id + AES-256 | Credentials encrypted at rest on your device |
| 📦 | **File Backup** | Argon2id + AES-256-GCM | Export encrypted \`.marix\` files with authenticated encryption |
| 🔄 | **Cloud Sync** | Argon2id + AES-256-GCM | Client-side encryption—cloud providers store only encrypted blobs |

---

## ⚡ Performance & Optimization

Marix is optimized to run smoothly on low-end machines:

### Auto-Tuned KDF (Best Practice)

Marix uses **auto-calibration** for Argon2id parameters—a widely-adopted best practice in applied cryptography:

| Feature | Description |
|---------|-------------|
| **Target Time** | ~1 second (800-1200ms) on user's machine |
| **Auto-Calibration** | Memory and iterations auto-tuned at first run |
| **Adaptive** | Works optimally on both weak and powerful machines |
| **Background Calibration** | Runs on app startup for seamless UX |
| **Stored Parameters** | KDF params saved with encrypted data for cross-machine decryption |
| **Security Floor** | Minimum 64MB memory, 2 iterations (exceeds OWASP 47MB) |

> **Why ~1 second?** This is the standard recommendation in practical cryptography. It provides strong brute-force resistance while remaining acceptable for user experience. Parameters adapt to each machine automatically—no need to guess "standard" settings.

### Memory Baseline (Starting Point for Auto-Tune)

| System RAM | Baseline Memory | Then Auto-Tuned |
|------------|-----------------|-----------------|
| ≥ 16 GB | 512 MB | → Calibrated to ~1s |
| ≥ 8 GB | 256 MB | → Calibrated to ~1s |
| ≥ 4 GB | 128 MB | → Calibrated to ~1s |
| < 4 GB | 64 MB | → Calibrated to ~1s |

### Runtime Optimizations

| Optimization | Technology | Benefit |
|--------------|------------|---------|
| **V8 Heap Limit** | \`--max-old-space-size=256MB\` | Prevents memory bloat |
| **Background Throttling** | \`--disable-renderer-backgrounding\` | Keeps connections alive |
| **Terminal Buffer** | Scrollback: 3,000 lines | 70% memory reduction vs default |
| **Lazy Loading** | On-demand component loading | Faster startup |
| **GC Hints** | Manual garbage collection triggers | Reduced memory footprint |

### Tech Stack

| Component | Technology | Purpose |
|-----------|------------|---------|
| **Framework** | Electron 39 + React 19 | Cross-platform desktop app |
| **Terminal** | xterm.js 6 | High-performance terminal emulation |
| **SSH/SFTP** | ssh2 + node-pty | Native SSH protocol implementation |
| **Code Editor** | CodeMirror 6 | Lightweight syntax highlighting |
| **Encryption** | Argon2 + Node.js Crypto | Strong client-side encryption |
| **Styling** | Tailwind CSS 4 | Modern, minimal CSS |
| **Build** | Webpack 5 + TypeScript 5 | Optimized production bundles |

---

## 📥 Download

<table>
<tr>
<td align="center" width="33%">
<img src="https://img.icons8.com/fluency/96/windows-10.png" width="64"><br>
<b>Windows</b><br>
<a href="https://github.com/user/marix/releases/latest/download/Marix-Setup.exe">Download .exe</a>
</td>
<td align="center" width="33%">
<img src="https://img.icons8.com/fluency/96/mac-os.png" width="64"><br>
<b>macOS</b><br>
<a href="https://github.com/user/marix/releases/latest/download/Marix-Intel.zip">Intel .zip</a><br>
<a href="https://github.com/user/marix/releases/latest/download/Marix-arm64.zip">Apple Silicon</a>
</td>
<td align="center" width="33%">
<img src="https://img.icons8.com/external-tal-revivo-color-tal-revivo/64/external-linux-a-family-of-open-source-unix-like-operating-systems-based-on-the-linux-kernel-logo-color-tal-revivo.png" width="64"><br>
<b>Linux</b><br>
<a href="https://github.com/user/marix/releases/latest/download/Marix.AppImage">.AppImage</a> •
<a href="https://github.com/user/marix/releases/latest/download/marix.deb">.deb</a> •
<a href="https://github.com/user/marix/releases/latest/download/marix.rpm">.rpm</a>
</td>
</tr>
</table>

> **⚠️ Linux Users:** When upgrading, use `sudo dpkg -r marix` to uninstall the old version before installing the new one. Using Software Center may leave old files behind.

---

## ✨ Features

### 🔌 Multi-Protocol Connections

| Protocol | Technology | Description |
|----------|------------|-------------|
| **SSH** | ssh2 + node-pty | Secure Shell with password & private key authentication |
| **SFTP** | ssh2 | Dual-pane file manager with drag-and-drop |
| **FTP/FTPS** | basic-ftp | Standard and secure FTP support |
| **RDP** | xfreerdp3 / mstsc | Remote Desktop (xfreerdp3 on Linux, mstsc on Windows) |
| **Database** | mysql2, pg, mongodb, redis, better-sqlite3 | Connect to MySQL, PostgreSQL, MongoDB, Redis, SQLite |

### 💻 Terminal

- **400+ color themes** - From Dracula to Solarized, Catppuccin, Nord, and more
- **Customizable fonts** - Any system font, any size
- **Full xterm.js 6** - Complete terminal emulation with Unicode support
- **Session preservation** - Tabs persist across reconnects
- **OS detection** - Auto-detect Linux distro & display system info

### 📁 SFTP File Manager

- **Dual-pane interface** - Local ↔ Remote side by side
- **Integrated editor** - CodeMirror 6 with 15+ language syntax highlighting
- **Drag & drop** - Upload/download files easily
- **Permission management** - chmod with visual interface
- **Batch operations** - Multi-select files for transfer
- **Compress/Extract** - Right-click to zip, tar.gz, extract archives

### 📦 Source Installer

*Install popular frameworks and CMS directly on your server via SSH.*

| Category | Frameworks |
|----------|------------|
| **PHP** | Laravel, WordPress, Symfony, CodeIgniter 3/4 |
| **JavaScript** | Express.js, NestJS, Fastify, Vue.js, Nuxt.js, React, Next.js |
| **TypeScript** | TypeScript Node projects |

**Features:**
- **Dynamic version fetching** - Versions fetched from GitHub/npm APIs in real-time
- **Auto-discover new versions** - Laravel 13+, WordPress 7+, etc. automatically detected
- **PHP version compatibility check** - Warns if server PHP version doesn't match requirements
- **Sub-version selection** - Choose specific patch versions (e.g., Laravel 11.5.0)
- **One-click installation** - Installs framework with all dependencies
- **Database configuration** - Auto-configure .env or wp-config.php

### 🛠️ Built-in Tools

<details>
<summary><b>LAN File Transfer</b></summary>

*Share files instantly between devices on your local network.*

- **Sender**: Select files → Show 6-digit code → Wait for receiver
- **Receiver**: Enter sender's code → Auto-find sender via UDP → Select save folder → Receive files
- Real-time transfer progress with speed display
- TCP-based reliable transfer (64KB chunks)
- Send multiple files and folders
- Works on WiFi and Ethernet

</details>

<details>
<summary><b>LAN Server Sharing</b></summary>

*Share server configurations with nearby devices securely.*

- **Sender**: Select servers → Show 6-digit code → Select peer device → Send encrypted data
- **Receiver**: Receive notification → Enter sender's code to decrypt → Import servers
- AES-256-CBC encryption with scrypt key derivation
- Auto-discover devices on local network
- Option to include/exclude passwords and private keys

</details>

<details>
<summary><b>DNS & Network Tools</b></summary>

- A / AAAA / MX / TXT / SPF / CNAME / NS / SOA / PTR lookups
- Ping & Traceroute
- TCP port testing
- HTTP/HTTPS checker with SSL info
- SMTP server testing
- IP Blacklist checker (10 RBLs)
- WHOIS lookup
- ARIN/IP ownership lookup
- Listening ports scanner

</details>

<details>
<summary><b>Cloudflare DNS Manager</b></summary>

*Optional built-in tool for managing Cloudflare DNS directly from your SSH workspace.*

- Manage all your domains
- Create/Edit/Delete DNS records
- Support for A, AAAA, CNAME, MX, TXT, SRV, CAA records
- Toggle Cloudflare proxy (orange cloud)
- TTL management
- **API key included in encrypted backups**

</details>

<details>
<summary><b>SSH Key Manager</b></summary>

- Generate RSA-4096, Ed25519, ECDSA-521 keys
- Import existing keys from file
- Export public/private keys
- Secure local storage in ~/.marix/ssh_keys
- Key fingerprint display
- **Keys included in encrypted backups**

</details>

<details>
<summary><b>Known Hosts Manager</b></summary>

- View all SSH fingerprints
- Import from host
- Remove untrusted hosts
- Verify host authenticity

</details>

<details>
<summary><b>Command Snippets</b></summary>

*Save and organize frequently-used commands with optional keyboard shortcuts.*

- **Quick command library** - Store commands you use often with descriptions
- **Keyboard shortcuts** - Assign `Ctrl+Shift+[key]` (or `Cmd+Shift+[key]` on Mac) for instant execution
- **Categories** - Organize by System, Docker, Git, Network, Database, or Custom
- **Scope-based** - Global snippets or specific to a host/group
- **Snippet Panel** - Side panel in terminal for quick access
- **Search & filter** - Find snippets by name, command, or tags
- **Included in backup** - Snippets are encrypted and backed up with your data

**How to use:**
1. Open **Snippets** menu from the sidebar
2. Click **Add** to create a new snippet
3. Enter name, command, category, and optional hotkey
4. In SSH terminal:
   - Click snippet in the panel to insert command
   - Or press `Ctrl+Shift+[key]` to execute with hotkey

</details>

### 🎨 User Experience

- **Dark & Light themes** - Follow system or toggle manually
- **14 languages** supported
- **Server tagging** - Organize with colored tags
- **Quick connect** - Cmd/Ctrl+K to search servers
- **Connection history** - Quick access to recent connections

---

## 💾 Backup & Restore

### How Encryption Works

All backups use **Argon2id** (winner of the Password Hashing Competition) and **AES-256-GCM** (authenticated encryption):

<p align="center">
  <img src="lang/flow.png" alt="Encryption Flow" width="800">
</p>

### What Gets Backed Up

| Data | Included | Encrypted |
|------|----------|-----------|
| Server list (hosts, ports, credentials) | ✅ | ✅ AES-256-GCM |
| SSH private keys | ✅ | ✅ AES-256-GCM |
| Cloudflare API token | ✅ | ✅ AES-256-GCM |
| Command snippets | ✅ | ✅ AES-256-GCM |
| 2FA TOTP entries | ✅ | ✅ AES-256-GCM |
| Port forwarding configs | ✅ | ✅ AES-256-GCM |
| App settings & preferences | ✅ | ✅ AES-256-GCM |
| Known hosts | ❌ | — |

### Security Guarantees

- 🔐 **Password never stored** — Not in the file, not on GitHub, not anywhere
- 🔒 **Client-side encryption** — All encryption happens locally before data leaves your device
- 🛡️ **Brute-force resistant** — Argon2id requires 64-512MB RAM per attempt (auto-adjusted)
- ✅ **Tamper-evident** — AES-GCM (AEAD) authentication detects any modification to encrypted data
- 🔄 **Cross-machine compatible** — Backup stores KDF parameters for portability

---

### Local Encrypted Backup

Export all your data as an encrypted \`.marix\` file:

1. **Go to Settings** → **Backup & Restore**
2. **Create password** meeting requirements:
   - Minimum 10 characters
   - 1 uppercase, 1 lowercase, 1 number, 1 special character
3. **Export** - File is encrypted before saving
4. **Store safely** - Keep the backup file and remember your password

### Google Drive Backup (Zero-Knowledge)

Securely sync your encrypted backup to your Google Drive:

#### Setup

> 📘 **Setup Guide**: [Google Drive Setup Documentation](docs/google/GOOGLE_DRIVE_SETUP.en.md)

> ℹ️ **Pre-packaged Version**: If you're using the pre-built release (AppImage, RPM, etc.), Google credentials are already included. You can skip step 1 and connect directly.

1. **Configure OAuth Credentials**:
   - Create a Google Cloud Project
   - Enable Google Drive API
   - Create OAuth 2.0 Client ID
   - Download credentials JSON file
   - Save as `src/main/services/google-credentials.json`

2. **Connect in Marix**:
   - Go to Settings → Backup & Restore → Google Drive
   - Click "Connect to Google Drive"
   - Browser opens for Google OAuth
   - Grant permissions
   - App receives secure token

3. **Create Backup**:
   - Enter encryption password (10+ characters)
   - Click "Create Backup"
   - File uploaded to "Marix Backups" folder on Drive

4. **Restore Backup**:
   - Click "Restore from Google Drive"
   - Enter your backup password
   - All servers and settings restored

#### How It Works

```
[Your Data] → [Argon2id + AES-256] → [Encrypted Blob] → [Google Drive]
                   ↑
            Your Password
            (never uploaded)
```

- ✅ **End-to-end encrypted** - Data encrypted before leaving your device
- ✅ **Zero-knowledge** - Google only sees encrypted blobs
- ✅ **Your keys only** - OAuth tokens stored locally
- ✅ **Private backup folder** - Files only accessible by your app

### GitHub Backup (Zero-Knowledge)

Securely sync your encrypted backup to a private GitHub repository:

#### Setup

1. **Login with GitHub**:
   - Go to Settings → Backup & Restore → GitHub Backup
   - Click "Login with GitHub"
   - A device code will appear in the app
   - Browser opens automatically - enter the code and authorize
   - Done! A private repository \`marix-backup\` is automatically created

2. **Backup**:
   - Click "Backup to GitHub"
   - Enter your backup password
   - Encrypted data is pushed to your repository

3. **Restore on another device**:
   - Install Marix
   - Login with GitHub (same steps as above)
   - Click "Restore from GitHub"
   - Enter your backup password to decrypt

#### Why GitHub is Safe

| Layer | Protection |
|-------|------------|
| **Client-side encryption** | Data encrypted before leaving device |
| **Argon2id KDF** | 64-512MB memory (auto), 4 iterations, 1-4 parallel lanes |
| **AES-256-GCM** | AEAD with random IV (tamper-evident) |
| **GitHub storage** | Only encrypted ciphertext stored |
| **No Marix server** | Direct client ↔ GitHub communication |

> ⚠️ **Important**: If you lose your backup password, your backup is **permanently unrecoverable**. We cannot decrypt it. No one can.

---

## 🛡️ Security Specifications

### Encryption Details

| Component | Algorithm | Parameters |
|-----------|-----------|------------|
| Key Derivation | Argon2id | 64-512MB memory (auto), 4 iterations, 1-4 lanes |
| Encryption | AES-256-GCM | 256-bit key, AEAD (tamper-evident) |
| Salt | CSPRNG | 32 bytes per backup |
| IV/Nonce | CSPRNG | 16 bytes per operation |
| Auth Tag | GCM | 16 bytes |

> **Argon2id Parameters (OWASP 2024 compliant)**:
> - Memory: 64-512 MB (adaptive, minimum 64MB exceeds OWASP's 47MB recommendation)
> - Iterations: 4 (within OWASP's 3-5 range)
> - Parallelism: 1-4 (based on CPU cores)

### SSH Key Algorithms

| Algorithm | Key Size | Use Case |
|-----------|----------|----------|
| Ed25519 | 256-bit | Recommended (fast, secure) |
| RSA | 4096-bit | Legacy compatibility |
| ECDSA | 521-bit | Alternative to Ed25519 |

### Password Requirements

Your backup password must contain:
- ✅ Minimum 10 characters
- ✅ At least 1 uppercase letter (A-Z)
- ✅ At least 1 lowercase letter (a-z)
- ✅ At least 1 number (0-9)
- ✅ At least 1 special character (!@#\$%^&*...)

---

## 🔧 Build from Source

\`\`\`bash
# Clone repository
git clone https://github.com/marixdev/marix.git
cd marix

# Install dependencies
npm install

# Development
npm run dev

# Build
npm run build

# Package for distribution
npm run package:win    # Windows (.exe)
npm run package:mac    # macOS (.zip)
npm run package:linux  # Linux (.AppImage, .deb, .rpm)
\`\`\`

### Build Linux Releases on Windows

Build Linux release files from Windows with Docker Desktop instead of running `npm run package:linux` directly on Windows. AppImage packaging needs Linux tools such as `mksquashfs`, and native modules should be rebuilt for Linux.

PowerShell:

\`\`\`powershell
cd C:\projects\marix

docker pull electronuserland/builder:latest

docker run --rm `
  -v "${PWD}:/project" `
  -v marix-node-modules-linux:/project/node_modules `
  -v marix-npm-cache-linux:/root/.npm `
  -v marix-electron-cache-linux:/root/.cache/electron `
  -v marix-electron-builder-cache-linux:/root/.cache/electron-builder `
  -w /project `
  electronuserland/builder:latest `
  /bin/bash -lc "npm ci && npm run package:linux"
\`\`\`

Git Bash:

\`\`\`bash
cd /c/projects/marix

MSYS_NO_PATHCONV=1 docker run --rm \
  -v "$(pwd):/project" \
  -v marix-node-modules-linux:/project/node_modules \
  -v marix-npm-cache-linux:/root/.npm \
  -v marix-electron-cache-linux:/root/.cache/electron \
  -v marix-electron-builder-cache-linux:/root/.cache/electron-builder \
  -w /project \
  electronuserland/builder:latest \
  /bin/bash -lc "npm ci && npm run package:linux"
\`\`\`

Outputs are written to `release/`: `Marix-<version>.AppImage`, `marix_<version>_amd64.deb`, and `marix-<version>.x86_64.rpm`.

### System Requirements

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| OS | Windows 10, macOS 10.15, Ubuntu 20.04 | Latest |
| RAM | 2 GB | 4 GB+ |
| Storage | 200 MB | 500 MB |

### Linux RDP Dependencies

\`\`\`bash
# Ubuntu/Debian
sudo apt install freerdp3-x11 xdotool

# Fedora
sudo dnf install freerdp xdotool

# Arch
sudo pacman -S freerdp xdotool
\`\`\`

---

## 📄 License

This project is licensed under the **GNU General Public License v3.0** (GPL-3.0).

This means:
- ✅ You can use, modify, and distribute this software
- ✅ You can use it for commercial purposes
- ⚠️ Any modifications must also be released under GPL-3.0
- ⚠️ You must disclose source code when distributing
- ⚠️ You must state changes made to the code

See [LICENSE](LICENSE) for the full license text.

---

<p align="center">
  <strong>Marix</strong> — A modern, zero-knowledge SSH client<br>
  <em>Your data. Your responsibility. Your freedom.</em><br><br>
  <sub>If you want convenience at the cost of privacy, Marix is not for you.</sub>
</p>
