# RAUF × LUCIFER MD BOT

A free, multi-device WhatsApp bot with 600+ commands, a companion Telegram bot and a self-hosted control website. Built with Node.js and [Baileys](https://github.com/WhiskeySockets/Baileys).

- **Pairing code login** – link a number with an 8-character code from the website or Telegram; no QR scan.
- **600+ commands** – media downloads, AI assistant, stickers, group administration, games and utilities (`.menu` lists them).
- **White-label** – users can create their own branded bot (name, owner, prefix, logo) from the website.
- **Control website** – responsive liquid-glass UI, command browser, AI support chat, live preview, optional password gate.
- **Admin panel** – health, backups, sessions, branding and zip upgrades at a private URL.
- **Voice agent (Arya)** – optional outbound voice replies, see [docs/voice-agent.md](docs/voice-agent.md).

**Version:** 25.1 BETA

> Not affiliated with, endorsed by or sponsored by WhatsApp or Meta.

## Requirements
- Node.js 18 or newer (tested on 22), ffmpeg on `PATH` for media commands
- A public HTTPS address for the website (own domain, Cloudflare Tunnel or ngrok)

## Installation
```bash
npm install --legacy-peer-deps --no-audit --no-fund
cp .env.example .env          # then edit .env
cp ngrok.txt.example ngrok.txt   # only if you use ngrok
npm start
```
The server listens on `PAIR_PORT`, `SERVER_PORT` or `PORT` (default 2159).

## Configuration
All settings are environment variables in `.env`.

| Variable | Purpose |
|---|---|
| `PAIRING_NUMBER` | Owner number (digits, with country code). Leave empty to skip owner pairing; the website and Telegram still run. |
| `OWNER_NUMBER`, `OWNER_NUMBER_2` | Owner numbers with owner-only command access. |
| `ADMIN_CODE` | Secret path of the admin panel: `https://your-domain/<ADMIN_CODE>`. Use a long random value. |
| `TELEGRAM_BOT_TOKEN` | Telegram session bot. |
| `TELEGRAM_PASSWORD_BOT_TOKEN` | Bot that issues website passwords when the gate is on. |
| `TELEGRAM_ADMIN_IDS` | Comma-separated Telegram IDs allowed to use admin features. |
| `SITE_DOMAIN` | Your public domain. |
| `CLOUDFLARE_TUNNEL_TOKEN` | Starts a Cloudflare Tunnel automatically. |
| `NGROK_AUTHTOKEN`, `NGROK_DOMAIN` | Alternative to Cloudflare (or put them in `ngrok.txt`). |
| `GROQ_API_KEY` | Speech-to-text for the voice agent. |

See `.env.example` for the complete list.

## Project layout
```
index.js, main.js     entry points
commands/             bot commands
lib/                  server, pairing, admin panel, Telegram, branding, tunnel
public/               website (static SPA, service worker)
features/             optional feature modules
scripts/              checks and offline tests
docs/                 guides and reports
```

## Admin panel
Open `https://your-domain/<ADMIN_CODE>`. Repeated wrong codes are rate-limited. From there you can view health, download backups, edit branding and upgrade from a zip (sessions, `.env` and `ngrok.txt` are never overwritten).

## Search and AI discoverability
The server publishes `/robots.txt`, `/sitemap.xml` and `/llms.txt`, plus JSON-LD and Open Graph metadata, so search engines, AI assistants and link previews can identify the project. When the website password gate is enabled, crawlers still receive a public description page.

## Testing
```bash
npm run check            # syntax check of every source file
npm test                 # check + smoke test
npm run test:security    # security suite
npm run test:telegram    # Telegram flow
```
All tests run offline.

## Security
Never publish `.env`, `ngrok.txt`, `session/` or `data/`. See [SECURITY.md](SECURITY.md).

## Publishing on GitHub
See [GITHUB-UPLOAD.md](GITHUB-UPLOAD.md). Never publish `.env`, `ngrok.txt`, `session/` or `data/`.

## License
ISC, see [LICENSE](LICENSE).
