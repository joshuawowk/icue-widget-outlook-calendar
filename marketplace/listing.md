# Maker Console listing: Agenda for Outlook Calendar

Copy-paste kit for the Elgato Maker Console (https://maker.elgato.com). Values below were
drafted against the Elgato product guidelines (https://docs.elgato.com/guidelines/products)
as researched in October 2026; re-check the guidelines before submitting, because limits can change.

## Product name

Agenda for Outlook Calendar

(27 characters; the limit is 30. "for Outlook Calendar" wording marks this as a third-party widget that works with the service; it does not use the word "Official" or any Microsoft or Outlook logo. The manifest `name` is "Outlook Calendar"; the listing name deliberately puts the service after "for". The widget icon is a neutral calendar glyph, not the Outlook logo.)

## Tagline

An agenda-style Outlook calendar with a next-event countdown for the Xeneon Edge, read through a companion service you run.

(123 characters.)

## Description

See your Outlook calendar as an agenda on the Xeneon Edge. Events for the next one to fourteen days are grouped under day separators such as Today and Tomorrow, each with an accent time tile, the time range and the location. The header shows today's date and a live countdown to your next event, like 25m or 2h 15m, which turns into a red busy marker during a meeting, and the event happening now is highlighted. You can hide all-day events, and the last good data stays on screen with an offline tag if the source is unreachable. The widget does not talk to Microsoft directly. It reads normalized JSON from xeneon-graph-proxy, a companion service you run yourself, which can be fed by Microsoft Graph device-code sign-in or by a browser-scrape ingest for tenants that block app registrations. Two or three agenda columns appear on large horizontal slots, and colors, background image, glass blur and transparency are adjustable. Requires iCUE 5.52 or later on Windows, a Xeneon Edge and a running xeneon-graph-proxy with calendar data; its setup is in the GitHub README. This is an independent, unofficial widget, not affiliated with Microsoft. It is free software under the MIT license.

(1189 characters; Maker Console asks for 250 to 1500. Counted with Python `len()` on the paragraph above.)

## Category

Productivity. (The widget is a calendar agenda; the closest observed category is Productivity.) The exact Maker Console category list for iCUE widgets was not verified when this kit was
written. The categories observed live on the Elgato Marketplace iCUE widgets page in October 2026 were Audio, Productivity,
Gaming, Artistic, Monitoring, Engagement and Social; pick the closest one the console offers.

## Release notes

Version 1.0.1

- Declares url network permissions in the manifest (`localhost:8585` and `any`).
- Sets `min_app_version` to 5.52.
- Bundles the media viewer inside the widget folder and self-closes the head elements, so the package passes the iCUE widget validator.
- Replaces personal host defaults with generic ones.
- Adds a SECURITY policy and a README Install section that points at the GitHub release.

(Condensed from the GitHub release notes for v1.0.1 (`gh release view v1.0.1 -R joshuawowk/icue-widget-outlook-calendar`).)

## Requirements

- CORSAIR iCUE 5.52 or later (the manifest sets `min_app_version` to 5.52).
- Windows PC (the manifest targets `os` windows).
- A CORSAIR Xeneon Edge (`dashboard_lcd` device).
- xeneon-graph-proxy (https://github.com/joshuawowk/xeneon-graph-proxy) running and reachable from the PC, by default at `http://localhost:8585`. It is a companion service the user runs; the widget does not talk to Microsoft directly.
- Calendar data in the proxy, fed either by Microsoft Graph device-code sign-in (if the tenant permits public clients) or by the browser-scrape ingest described in the widget README (a scheduled browser session posts normalized JSON to the proxy's `/ingest/calendar` endpoint).
- Packaging status of the proxy is a snapshot: the v1.0.1 README says the `ghcr.io` container image was not yet published and the proxy must be built from source with `docker compose up -d --build`. Re-check with `gh release list -R joshuawowk/xeneon-graph-proxy` and the proxy README before submitting, and update this listing if the image is now published.

## Network permissions

The manifest declares two permissions: `{ "type": "url", "domain": "localhost", "port": 8585 }` for the default proxy address, and `{ "type": "url", "domain": "any" }`, the iCUE spec's wildcard for all external hosts, because the Proxy URL setting can point at any host on the user's network. The widget talks only to the user's own proxy; it contacts no third party. If the proxy runs on a different localhost port, iCUE asks for permission at runtime and the user clicks Allow. iCUE shows the permissions at install, and they can be revoked in the widget settings.

## License

MIT, see `LICENSE` in the repository root. Source: https://github.com/joshuawowk/icue-widget-outlook-calendar

## Submission notes

- Asset specification unverified. The iCUE-specific listing asset requirements (icon size, gallery image count and
  dimensions, description limits) were not found in Elgato's documentation when this kit was written. This kit reuses the
  sizes of the Stream Deck kit: a 288x288 listing icon and 1920x960 gallery images, with a description of 250 to 1500
  characters. Re-check the Maker Console form and resize or re-render (`marketplace/build.sh`) if it asks for something else.
- Submission route unverified. Elgato's become-a-maker page lists Stream Deck, Wave Link, OBS and 3D as self-serve Maker
  Console categories but does not list iCUE widgets. If the console has no iCUE widget option, the fallback is the
  maker@elgato.com "New Product Submission" email route.
- File to upload: `outlook-calendar.icuewidget`, the `.icuewidget` attached to GitHub release v1.0.1:
  https://github.com/joshuawowk/icue-widget-outlook-calendar/releases/download/v1.0.1/outlook-calendar.icuewidget
  Download it from the release; do not zip the folder by hand. To rebuild it, use `npm run package` (needs
  `npm install -g icuewidget-cli`).
- Manifest version is 1.0.1 and the file above matches release v1.0.1. If you submit a later release, update the
  version, release notes and file name in this kit first.
- Listing icon: `resources/icon.png` is 256x256 and was upscaled to 288x288 with ImageMagick (`convert resources/icon.png -resize 288x288!`), so it is slightly soft; re-export `resources/icon.svg` at 288 for a sharper icon if needed.
- Packaging caveat: `npm run package` runs `icuewidget package .` on the repository root, which zips every non-hidden file. A `marketplace/` folder would therefore be swept into future `.icuewidget` builds (about 0.8 MB of PNGs and HTML). Nothing in this kit changes the package script; stage the widget files or exclude `marketplace/` before the next release build.
- Validator note: `icuewidget validate` (CLI 0.5.0, run in October 2026 on the unzipped local `dist/` build of that version) passes with one warning that the `any` url permission grants access to every external host. The permission is intentional because the host is user-entered; expect reviewers to ask about it and point them at the Network permissions section.
- Not tested on hardware: the kit was written without Windows, iCUE or a Xeneon Edge. Gallery slides show the shipped preview and text only; no screenshot was taken from a running panel.

## Assets in this folder

| File | Size | Use |
| --- | --- | --- |
| `listing-icon-288.png` | 288x288 | Product icon |
| `gallery-01.png` | 1920x960 | Overview: product name and a crop of the widget header and first agenda item from `resources/preview.png` |
| `gallery-02.png` | 1920x960 | Feature cards: day grouping, countdown, busy marker, all-day filter, offline, sizing |
| `gallery-03.png` | 1920x960 | Widget, xeneon-graph-proxy and the calendar source, setup steps and network permission |

`resources/preview.png` is NOT used in full. Its lower rows contain what look like real calendar entries (including a work project name), and the gallery images must carry no real calendar content, so slide 1 uses only a crop of the header and the first generic agenda item ("Focus block") and the other slides are built from the feature list. `resources/preview.png` is also shipped inside the released `.icuewidget`; consider regenerating it from sample data.
`marketplace/src/` holds the HTML sources and `marketplace/build.sh` re-renders the gallery PNGs with
headless Chromium.

## Trademark note

"Microsoft", "Outlook" and "Microsoft Graph" are trademarks of Microsoft Corporation. This widget is independent and unofficial, is not affiliated with or endorsed by Microsoft, and uses no Microsoft logo.
