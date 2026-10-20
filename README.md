# Outlook Calendar — iCUE Widget for Xeneon Edge

[![Release](https://img.shields.io/github/v/release/joshuawowk/icue-widget-outlook-calendar)](https://github.com/joshuawowk/icue-widget-outlook-calendar/releases/latest)
[![License: MIT](https://img.shields.io/github/license/joshuawowk/icue-widget-outlook-calendar)](LICENSE)

An agenda-style Outlook calendar for the CORSAIR Xeneon Edge: dark theme, accent **time tiles** per event, day separators (*Today / Tomorrow / Jun 12 • FRIDAY*), locations, a highlighted *happening now* row, and a **next-event countdown** in the header.

![preview](resources/preview.png)

> **Requires a companion service.** This widget does not talk to Microsoft directly. It reads from **[xeneon-graph-proxy](https://github.com/joshuawowk/xeneon-graph-proxy)**, a small service you run yourself that signs in to Microsoft Graph and serves read-only JSON for your Outlook calendar. Set that up first:
>
> ```bash
> docker run -d --name xeneon-graph-proxy --restart unless-stopped -p 8585:8585 -v xeneon-graph-proxy-data:/data ghcr.io/joshuawowk/xeneon-graph-proxy:latest
> ```
>
> Run the proxy from its published image, `ghcr.io/joshuawowk/xeneon-graph-proxy` (linux/amd64, linux/arm64); the [proxy README](https://github.com/joshuawowk/xeneon-graph-proxy#quick-start-docker) has the full `docker run` and compose setup. Then start sign-in once with `curl -X POST http://localhost:8585/auth/start` and follow the printed instructions.

Microsoft Graph has no static API keys, so this widget pairs with **[xeneon-graph-proxy](https://github.com/joshuawowk/xeneon-graph-proxy)**. The proxy can be fed two ways:

1. **Graph device-code sign-in** (if your tenant permits public clients)
2. **Browser-scrape ingest** — a scheduled browser session reads Outlook Web in your signed-in profile and POSTs normalized JSON to the proxy's `/ingest/calendar` endpoint. Useful when corporate tenants block OAuth app registrations.

## Features

- Agenda for the next 1–14 days, grouped by day with locale-aware times
- Header: today's date + live countdown to the next event ("25m", "2h 15m"); turns red **busy** during a meeting
- *Happening now* row highlight
- Optional hiding of all-day events
- Offline resilience (last good data with an `offline` tag)
- Responsive: all Xeneon Edge slots; two/three-column agenda on L/XL horizontal
- Personalization: colors, background image, glass blur, transparency

## Install

**Prerequisites**

- Windows PC with CORSAIR iCUE 5.52 or newer and a Xeneon Edge
- [xeneon-graph-proxy](https://github.com/joshuawowk/xeneon-graph-proxy) running and reachable from the PC (see the note above)

**Steps**

1. Download `outlook-calendar.icuewidget` from the [latest release](https://github.com/joshuawowk/icue-widget-outlook-calendar/releases/latest).
2. Double-click the file, or in iCUE open **Widgets**, click **+**, and select the file.
3. Add the widget to a Xeneon Edge slot.
4. Set **Proxy URL** if your proxy is not at `http://localhost:8585`.
5. Make sure the proxy has calendar data, via Graph sign-in or the scrape ingest described above.

**Network permission.** The manifest declares `{ "type": "url", "domain": "localhost", "port": 8585 }` for the default proxy address, plus the spec's `{ "type": "url", "domain": "any" }` wildcard because the **Proxy URL** setting can point at any host on your network. iCUE shows these at install and you can revoke them in the widget settings. If the proxy runs on a different `localhost` port, iCUE will ask for permission at runtime; click Allow.

## Settings

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| Proxy URL | text | `http://localhost:8585` | Where xeneon-graph-proxy runs |
| Days Ahead | slider | 5 | 1–14 |
| Hide All-Day Events | switch | off | |
| Refresh Interval | slider | 5 min | 1–30 min |
| Text / Accent / Background Color | color | white / `#3b82f6` / `#15161c` | |
| Background Image + Glass Blur | media | — | |
| Background Transparency | slider | 100% | |

## States

Loading · Waiting for data (proxy has no calendar yet) · Empty ("Nothing scheduled") · Error (proxy unreachable) · Content

## Development

Requires Node.js and the official CLI (`npm install -g icuewidget-cli`, tested with 0.5.0).

```bash
npm run validate   # icuewidget validate .
npm run package    # icuewidget package . -o dist/outlook-calendar.icuewidget
```

Always build the package with `icuewidget package` (it validates first); do not zip the folder by hand. `dist/` is gitignored.

## License

MIT
