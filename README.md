# Cloudflare Tunnel client for mipsle devices

[![Build & Release cloudflared (mipsle)](https://github.com/Le-Maxime/cloudflared/actions/workflows/build.yml/badge.svg)](https://github.com/Le-Maxime/cloudflared/actions/workflows/build.yml)

Tested on Keenetic Viva (KN-1910)

Current version `2026.10.7`

Builded with Go `1.27.3`

WARNING: All software provided here is provided as is without any questions or complaints. Please direct all questions and complaints to the original cloudflared repository, this is just an assembly for mipsle devices assembled from the source code without any changes. All open issues related to the work of cloudflared itself will be closed without response. You have been warned.

Compiled binary from https://github.com/cloudflare/cloudflared to mipsle devices.
Tested on Keenetic Viva (KN-1910) (Keenetic OS 4.2 Beta 3) with Entware.

## Usage

Connect to OPKG SSH:

```sh
opkg install wget-ssl
# (if wget already exists and complains about http/ftp connection: opkg remove wget-nossl)

wget -O /opt/etc/init.d/S99cloudflared https://raw.githubusercontent.com/Le-Maxime/cloudflared/main/S99cloudflared

nano /opt/etc/init.d/S99cloudflared
# Replace %yourtokenhere% with your token obtained at Cloudflare Zero Trust Dashboard -> Networks -> Tunnels

chmod +x /opt/etc/init.d/S99cloudflared

# Download binary (from main branch):
wget -O /opt/home/cloudflared https://github.com/Le-Maxime/cloudflared/raw/main/cloudflared
# Or download directly from Releases:
# wget -O /opt/home/cloudflared https://github.com/Le-Maxime/cloudflared/releases/latest/download/cloudflared-linux-mipsle

chmod +x /opt/home/cloudflared
```

After that, just reboot the router (after 60 seconds cloudflared will be online and visible in Cloudflare Zero Trust Dashboard), or start it manually:

```sh
/opt/etc/init.d/S99cloudflared start
```

---

## Automated Builds (GitHub Actions)

This repository includes a GitHub Actions workflow (`.github/workflows/build.yml`) that:
- Runs automatically on a daily schedule to detect new upstream releases in [cloudflare/cloudflared](https://github.com/cloudflare/cloudflared).
- Cross-compiles `cloudflared` for `linux/mipsle` (`GOMIPS=softfloat`).
- Strips debug symbols (`-s -w -trimpath`) for minimal binary size and router compatibility.
- Creates a new [GitHub Release](https://github.com/Le-Maxime/cloudflared/releases) with binary assets and SHA256 checksums.
- Automatically updates `cloudflared` in the root of the `main` branch to keep direct `wget` URLs working.
- Can be manually triggered at any time via **Actions** -> **Run workflow** (with optional custom version input).

---

## Build yourself

If you want to build it yourself, install Go first:

```sh
git clone https://github.com/cloudflare/cloudflared
cd cloudflared
CGO_ENABLED=0 GOOS=linux GOARCH=mipsle GOMIPS=softfloat go build -v -trimpath -ldflags="-s -w" -o cloudflared ./cmd/cloudflared
```
