# CDN Node Lookup

Browser-only CDN POP / Edge detector.  
Runs parallel probes against major CDNs, extracts node / POP identifiers from headers or trace endpoints, and provides a clean inspector + export tools.

**No backend. No API keys. Just open the HTML file.**

Inspired by https://ip.skk.moe/cdn-node-lookup

---

## Features

### Overview (the main test)
- ~28 built-in CDN endpoints plus **your own custom targets** (any HTTPS URL, with a POP header, Cloudflare-style trace, or body text), run with a configurable parallelism limit
- POP / node detection from headers or trace endpoints, mapped to a **city** (200+ airport codes plus vendor formats for Apple, CacheFly, CDN77, OVH, GCP). When a node doesn't reveal a location, the node IP or resolved endpoint is geolocated and marked `~` (approximate); anycast IPs are labelled `Anycast`
- **Cold vs warm latency** (1 / 3 / 5 samples): the table shows the warm median with cold and best below it, plus an inline bar
- **Routing efficiency**: great-circle distance from you to the POP, the physical minimum RTT over fibre, and measured ÷ ideal (`×`)
- **Route-change detection**: warns when a CDN's POP differs from your last saved run
- **IPv4 / IPv6** per endpoint (A / AAAA records via DNS-over-HTTPS, with the family your browser probably used highlighted) and your own v4 / v6 reachability
- Cache status (HIT / MISS / age) when exposed, protocol (h1 / h2 / h3), Resource Timing in the header inspector
- Clear status badges: `OK`, `OK · no POP`, `Blocked` (CORS), `Unreachable` (DNS/TLS/network), `Timeout`, `HTTP nnn`
- Search, filters (problems, route changed, China-focused, IPv4-only…), sortable columns, summary strip, **Retry failed**, **auto-refresh**
- Exports: TXT, JSON, **CSV**, copy report, and a **share link** (a compressed snapshot in the URL; it never includes your IP, ISP or location)
- Keyboard: `/` search, `r` re-run

### Live monitor
Browser-based `ping` for any set of targets: sparkline, last / min / avg / p95 / max, **jitter**, **packet loss**, and **POP-change alerts** with an event log and CSV export. Multi-CDN endpoints (jsDelivr) rotate backends by design and are not counted as route changes.

### Speed & bufferbloat
Cloudflare speed endpoints: idle latency and jitter, download and upload (1 / 4 / 8 streams), latency under load, **bufferbloat grade (A+ to F)** and packet loss. Also a throughput test against any CORS-enabled URL.

### DNS
Compare Cloudflare and Google resolvers for any hostname (latency cold / warm, CNAME chain, A / AAAA, TTL), detect your **actual system resolver** and EDNS subnet, and flag when resolvers return different answers (resolver-based steering). One click resolves every endpoint to show which CDN is behind each hostname.

### IPv4 vs IPv6
Forced-family probes (Cloudflare `1.1.1.1` literals, ipify, icanhazip, ident.me, Google) compare the two paths side by side, including whether Cloudflare serves each from a different POP.

### History & compare
The last 30 full runs are stored in your browser. Compare any two runs (or the current view) with per-CDN deltas, POP changes and trend sparklines. Import an exported JSON report to compare against another network, VPN or day.

---

## Supported CDNs

Akamai (standard, Video, Edge IP Binding), Apple Edge, AWS CloudFront, Bunny (Standard + Volume), ByteDance (Global + Overseas), CacheFly, CDN77, CDNetworks, Cloudflare, Cloudflare China Network, Fastly, GCP Anycast LB, jsDelivr (multi-CDN), Medianova, Melbicom / SwiftyCDN, NetEase, OVH CDN, QUANTIL, Tencent EdgeOne, Virtuozzo CDN, Zenlayer.

Latency-only (no POP header exposed to browsers): Google Cloud CDN, Microsoft Azure CDN, KeyCDN.

Gcore has no dedicated test: its previous test host stopped working, and `gcore.com` does not expose its POP header to browsers. It still shows up as a backend of the jsDelivr row.

## How It Works

The page performs **parallel HTTP requests** to CDN test endpoints and extracts POP / node identifiers from:

- Response headers (e.g. `x-served-by`, `x-amz-cf-pop`, `via`, etc.)
- CDN trace endpoints (e.g. Cloudflare `colo=`)
- Text-based edge endpoints (Apple Edge)
- Resource Timing (protocol, DNS, connect, TTFB) where the CDN sends `Timing-Allow-Origin`

All parsing happens locally in the browser.

---

## Usage

### Option 1 — Direct use

1. Download the HTML file  
2. Open it in your browser  
3. Results run automatically (or click **Start**)  

---

### Option 2 — Host it

You can host the file anywhere:

- GitHub Pages  
- Cloudflare Pages  
- Static web server  
- Local network  

No backend required.

---

## Exporting Results

- **TXT / JSON / CSV** — formatted report or raw data  
- **Copy** — copies the full report to clipboard  
- **Share** — copies a link that reproduces your results for someone else  
- **Header Inspector** — click *View* to see raw headers, timing and samples  

---

## Limitations

- Some CDNs hide POP headers (will show `Hidden / N/A`)  
- Browser CORS may block certain headers depending on network  
- Browsers cannot report which IP family each CDN request used; only client-side IPv4/IPv6 availability is shown  
- Distance / efficiency is an estimate: it needs a known POP location and your own location, and the ideal RTT is a physical lower bound  
- Cache status and POP headers are only readable when the CDN sends `Access-Control-Expose-Headers`  
- Results depend on your current network route / ISP / VPN  

---

## Privacy

- No tracking  
- No analytics  
- No data leaves your browser (except normal CDN test requests)  
- Third-party calls: `ipinfo.io` (your IP, ISP, location, and geolocation of CDN IPs), `api.ipify.org` / `api6.ipify.org`, `cloudflare-dns.com` and `dns.google` (DNS-over-HTTPS), `edns.ip-api.com` (system resolver detection), `speed.cloudflare.com` (speed test, client POP)  
- History, custom targets and geolocation cache stay in your browser's `localStorage` and are never sent anywhere  
- Share links contain only measurements, never your IP, ISP or location  

---
