# CDN Node Lookup

Browser-only CDN POP / Edge detector.  
Runs parallel probes against major CDNs, extracts node / POP identifiers from headers or trace endpoints, and provides a clean inspector + export tools.

**No backend. No API keys. Just open the HTML file.**

Inspired by https://ip.skk.moe/cdn-node-lookup

---

## Features

- Browser-only (pure HTML + Vanilla JS), no build step
- Probes ~28 CDN endpoints with a configurable parallelism limit (4 / 6 / 12 / all)
- Detects CDN POP / node from headers or trace endpoints, and maps airport-style POP codes (e.g. `IAD`, `AKL`) to a city
- **Cold vs warm latency**: the first request includes DNS/TLS setup; follow-up samples (1 / 3 / 5, configurable) reuse the connection. The table shows the warm median, with cold and best underneath
- Cache status column (HIT / MISS / age) when the CDN exposes it to browsers
- Clear status badges: `OK`, `OK · no POP` (reachable but POP header hidden), `Blocked` (host answered but CORS stops reading it), `Unreachable` (DNS/TLS/network), `Timeout`, `HTTP nnn`
- Sortable columns, summary strip (fastest / slowest / median / failures) and **Retry failed**
- Run history saved in `localStorage` (last 10 runs) with per-CDN and average deltas against the previous run; **Clear history** wipes it
- Header Inspector popup (raw headers, samples and Resource Timing per CDN)
- TXT export, JSON export, copy-to-clipboard report
- Client info: public IP, separate IPv4 / IPv6 reachability, ASN and location
- Auto-run on load, progress bar, mobile layout

---

## Supported CDNs

- Akamai (standard, Video, Edge IP Binding)
- Apple Edge
- AWS CloudFront
- Bunny (Standard + Volume)
- ByteDance (Global + Overseas)
- CacheFly
- CDN77
- CDNetworks
- Cloudflare and Cloudflare China Network
- Fastly
- GCP Anycast LB
- jsDelivr (multi-CDN: Cloudflare / Fastly / Gcore)
- Medianova
- Melbicom / SwiftyCDN
- NetEase
- OVH CDN
- QUANTIL
- Tencent EdgeOne
- Virtuozzo CDN
- Zenlayer
- Latency-only (no POP header exposed to browsers): Google Cloud CDN, Microsoft Azure CDN, KeyCDN

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

- **Export TXT** — saves a formatted report  
- **Copy** — copies the full report to clipboard  
- **Header Inspector** — click *View* to see raw headers  

---

## Limitations

- Some CDNs hide POP headers (will show `Hidden / N/A`)  
- Browser CORS may block certain headers depending on network  
- Browsers cannot report which IP family each CDN request used; only client-side IPv4/IPv6 availability is shown  
- Cache status and POP headers are only readable when the CDN sends `Access-Control-Expose-Headers`  
- Results depend on your current network route / ISP / VPN  

---

## Privacy

- No tracking  
- No analytics  
- No data leaves your browser (except normal CDN test requests)  
- Public IP lookup uses `https://ipinfo.io`, and `api.ipify.org` / `api6.ipify.org` for IPv4 / IPv6  
- Run history stays in your browser's `localStorage` and is never sent anywhere  

---
