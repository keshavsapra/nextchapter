<div align="center">

<img src="frontend/public/static/logo.png" alt="NextChapter logo" width="140">

# NextChapter

### Your library. Your files. One gorgeous reader.

**A lightweight, self-hosted PDF & EPUB library with a blazing-fast full-text search engine and a beautiful built-in web reader.**

[![License](https://img.shields.io/badge/License-Apache_2.0-2563eb?style=flat-square&logo=apache&logoColor=white)](LICENSE)
[![CI](https://img.shields.io/github/actions/workflow/status/keshavsapra/nextchapter/docker.yml?branch=main&style=flat-square&logo=githubactions&label=CI)](https://github.com/keshavsapra/nextchapter/actions/workflows/docker.yml)
![Formats](https://img.shields.io/badge/Formats-PDF%20%7C%20EPUB-f97316?style=flat-square)
![Platforms](https://img.shields.io/badge/Arch-amd64%20%7C%20arm64-10b981?style=flat-square&logo=docker)
![Built with](https://img.shields.io/badge/Rust%20%2B%20Svelte-0891b2?style=flat-square&logo=rust)
[![Buy Me A Coffee](https://img.shields.io/badge/Buy%20Me%20a%20Coffee-Support%20the%20project-FFDD00?style=flat-square&logo=buy-me-a-coffee&logoColor=black)](https://buymeacoffee.com/ar_kenny)

<br>

<p align="center">
  <kbd><img src="screenshots/example.png" alt="NextChapter in action" width="86%"></kbd>
</p>

</div>

<br>

> [!IMPORTANT]
> **NextChapter** began as an enhanced fork of **[Inkheart](https://github.com/Nystik-gh/inkheart)** by [Nystik](https://github.com/Nystik-gh) and has grown into its own project — adding native **EPUB reading**, multi-arch CI/CD (`linux/amd64` + `linux/arm64`), nine curated themes, and a deeply reworked reader experience.

<br>

## 🧭 The idea

**Your filesystem is the source of truth.** No database. No metadata curation. No lock-in.

Drop your files into folders, point NextChapter at the directory, and it:

1. 📂 Mirrors your exact folder hierarchy
2. 🖼️ Extracts high-resolution cover art automatically
3. 🔍 Builds an instant full-text search index (PDF *pages* and EPUB *chapters*)
4. 📖 Serves it all through a sleek web reader — desktop, tablet, and mobile

It deliberately does one thing exceptionally well: a **fast, beautiful way to read and search your own documents**. It's not a comic scraper, not an OPDS manager, not a metadata librarian.

<br>

## ✨ Highlights

<table>
<tr><th width="50%">📚 Library</th><th width="50%">📖 Readers</th></tr>
<tr><td>

- **Filesystem-driven** — your folder tree *is* the library, zero redundancy
- **Full-text search** — powered by [Tantivy](https://github.com/quickwit-oss/tantivy); searches inside PDF pages and EPUB chapters with heading boosts, and only re-indexes files that changed (`mtime`)
- **Smart covers** — automatic cover extraction from PDF first pages & EPUB metadata, with graceful fallback
- **Pins & Collections** — pin favourite folders to the sidebar, group documents into custom collections
- **OpenGraph previews** — share a link to any page or chapter and get a rich preview card

</td><td>

- **PDF** — PDF.js engine with continuous scroll, page spreads, thumbnails, and outline navigation
- **EPUB** — epub.js engine with TOC sidebar, paginated or continuous-scroll flow, and a location pill (`Page n / m`)
- **Keyboard & touch** — `←` `→` `Space` `PgUp` `PgDn` page turns, swipe on mobile, safe with open dialogs
- **Progress that sticks** — resumes exactly where you left off, down to the CFI location, saved locally
- **Deep links** — `/reader/:id?page=4` or any CFI anchor for direct linking

</td></tr>
<tr><th>🎨 Appearance</th><th>🛡️ Operations</th></tr>
<tr><td>

- **9 curated themes** — Light, Dark, Glassmorphism, Neomorphism, Aurora, Neo-Brutalism, Cyberpunk, Monochrome + a `system` mode
- **Theme the book, not just the app** — reader canvas follows the selected theme reliably, even after switching back and forth
- **Per-theme color scheme** — native controls, scrollbars, and form inputs match too
- **Mobile-first responsive** — drawer sidebar, auto-collapsing toolbars, touch navigation

</td><td>

- **Single static binary + SPA** — Rust (Actix-Web) backend, Svelte frontend, distroless container
- **Multi-arch images** — `linux/amd64` & `linux/arm64`: Raspberry Pi 4/5, Apple Silicon, NAS, VPS — all native
- **Runs as non-root** — UID 1000 inside the container
- **Optional auth** — drop-in Firebase Auth gate with account whitelisting
- **Reverse-proxy friendly** — sub-path deployments via `BASE_PATH`

</td></tr>
</table>

<br>

## 🚀 Quickstart

Create a `docker-compose.yml`:

```yaml
services:
  nextchapter:
    image: ghcr.io/keshavsapra/nextchapter:latest
    container_name: nextchapter
    restart: unless-stopped
    ports:
      - "8080:8080"
    volumes:
      - /path/to/your/books:/media         # Your PDFs and EPUBs
      - /path/to/covers:/covers            # Extracted cover images
      - /path/to/thumbnails:/thumbnails    # OpenGraph preview thumbnails
      - /path/to/config:/config            # Search index, settings, and pins
    environment:
      - BIND_ADDR=0.0.0.0
      - BIND_PORT=8080
      - SCAN_INTERVAL=600                  # Filesystem re-scan interval in seconds
      - TEXT_INDEX_ENABLED=true            # Enable full-text search indexing
```

Ensure the storage directories exist with write permissions for the container's non-root user (UID 1000):

```bash
mkdir -p /path/to/{covers,thumbnails,config,books}
sudo chown -R 1000:1000 /path/to/covers /path/to/thumbnails /path/to/config
```

> [!TIP]
> Alternatively, set `user: "0:0"` in the compose file to run as root on the host.

Then:

```bash
docker compose up -d
```

…and open **<http://localhost:8080>**.

> [!NOTE]
> The first index build over a large collection can take a few minutes. Since `/config` is persisted, later restarts only scan for new or modified files.

<br>

## 💻 Cross-platform deployment

Images are published as **multi-architecture** containers (`linux/amd64`, `linux/arm64`) — Docker picks the right one automatically:

| Platform | Works with |
|---|---|
| Linux / VPS | Ubuntu, Debian, Fedora, Arch, Alpine |
| Home servers / NAS | Synology, Unraid, TrueNAS SCALE, QNAP, Portainer |
| Single-board | Raspberry Pi 4B / 5, Rock Pi, Orange Pi |
| macOS / Windows | Docker Desktop, including native Apple Silicon |

### Docker CLI

```bash
docker run -d \
  --name nextchapter \
  --restart unless-stopped \
  -p 8080:8080 \
  -v /path/to/your/books:/media \
  -v /path/to/covers:/covers \
  -v /path/to/thumbnails:/thumbnails \
  -v /path/to/config:/config \
  ghcr.io/keshavsapra/nextchapter:latest
```

<br>

## ⚙️ Configuration

Configure via environment variables or a `nextchapter.toml` (legacy `inkheart.toml` also accepted).

| Environment Variable | Description | Default |
|---|---|---|
| `MEDIA_DIR` | Directory containing your PDF and EPUB files | `/media` |
| `IMAGE_DIR` | Directory where book covers are cached | `/covers` |
| `THUMBNAIL_DIR` | Directory for on-demand page preview thumbnails | `/thumbnails` |
| `CONFIG_DIR` | Directory storing user data, pins, collections, and search index | Parent dir of `CONFIG_PATH` |
| `CONFIG_PATH` | Optional path to a `nextchapter.toml` file | `./nextchapter.toml` |
| `BIND_ADDR` | Network address the server binds to | `127.0.0.1` (`0.0.0.0` in Docker) |
| `BIND_PORT` | HTTP port | `8080` |
| `SCAN_INTERVAL` | Filesystem scan interval for changes (in seconds) | `600` (10 min) |
| `COVER_WIDTH` | Width in pixels for extracted cover art | `900` |
| `THUMBNAIL_WIDTH` | Width in pixels for preview thumbnails | `300` |
| `TEXT_INDEX_ENABLED` | Enables full-text search indexing | `true` |
| `BASE_PATH` | Subdirectory path prefix (e.g. `/books`) when behind reverse proxy | `""` |
| `FIREBASE_CONFIG_PATH` | Path to `firebase.json` for optional Google/email auth | `./firebase.json` |
| `FIREBASE_WHITELIST` | Path to text file with whitelisted user IDs | None |

<br>

## 🌐 Reverse proxy & subdirectory setup

### Root domain

```nginx
location / {
    proxy_pass http://127.0.0.1:8080;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
}
```

### Subdirectory (e.g. `/library/`)

Set `BASE_PATH=/library` in the container environment, then:

```nginx
location /library/ {
    proxy_pass http://127.0.0.1:8080/;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
}
```

Works the same with Caddy, Traefik, or Cloudflare Tunnels.

<br>

## 🛠️ Building from source

### Local Docker build

```bash
docker build -t nextchapter:local -f docker/dockerfile .
```

### CI/CD

Every push triggers [`.github/workflows/docker.yml`](.github/workflows/docker.yml), which builds multi-arch images (`linux/amd64`, `linux/arm64`) and publishes them to GitHub Container Registry (`ghcr.io`).

<br>

## 🏗️ Architecture

```
nextchapter/
├── backend/            # Rust Actix-Web server
│   ├── indexer/        # Filesystem scanner, PDF/EPUB cover & chapter extraction
│   │   └── text_search/# Tantivy search engine, mtime change tracking, HTML parser
│   ├── server/         # REST API routes, auth extractor, static file serving
│   └── pdfium/         # Native PDFium bindings for PDF rendering
├── frontend/           # Svelte 4 SPA
│   ├── src/ui/         # NativePdf (PDF.js) & EpubReader (epub.js) components
│   └── src/stores/     # LocalStorage reading progress, themes, collections, pins
└── docker/             # Multi-stage multi-arch Dockerfile (cargo-chef + node + distroless)
```

<br>

## 📸 Screenshots

<p align="center">
  <img src="screenshots/folders.png" width="31%" alt="Folders">
  <img src="screenshots/files.png" width="31%" alt="Files">
  <img src="screenshots/reader.png" width="31%" alt="Reader">
</p>
<p align="center">
  <img src="screenshots/reader-spread.png" width="48%" alt="Reader spread">
  <img src="screenshots/opengraph-preview.png" width="48%" alt="OpenGraph link preview">
</p>

<br>

## 📄 License & credits

- **Inkheart** was originally created by [Nystik](https://github.com/Nystik-gh) ([original repo](https://github.com/Nystik-gh/inkheart) · [support Nystik on Ko-fi](https://ko-fi.com/nystik))
- EPUB support, multi-architecture CI/CD, and all further enhancements by [Keshav Sapra](https://github.com/keshavsapra)
- Licensed under the [Apache License, Version 2.0](LICENSE) — free to use, modify, and distribute under its terms
- Logo and branding assets are copyright of their respective authors

<div align="center">
  <br>
  <sub>Built with 🦀 Rust, ⚡ Svelte, and 📚 far too many unread books</sub>
</div>
