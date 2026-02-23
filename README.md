# CDN Node Lookup

Browser-only CDN POP / Edge detector.  
Runs parallel probes against major CDNs, extracts node / POP identifiers from headers or trace endpoints, and provides a clean inspector + export tools.

**No backend. No API keys. Just open the HTML file.**

Inspired by https://ip.skk.moe/cdn-node-lookup

---

## Features

- Browser-only (pure HTML + Vanilla JS)
- Parallel ultra-fast scanning (~24 CDN tests)
- Detects CDN POP / node from headers or trace endpoints
- Apple Edge support (`test.edge.apple`)
- Cloudflare trace support (`/cdn-cgi/trace`)
- Header Inspector popup (full raw headers per CDN)
- TXT export of results
- Copy-to-clipboard report
- Auto-run on load
- Progress bar
- Modern dark UI
- No tracking, no external dependencies

---

## Supported CDNs

- Akamai  
- Akamai Video  
- Akamai Edge IP Binding  
- Apple Edge  
- AWS CloudFront  
- Bunny (Standard + Volume)  
- ByteDance (Global + Overseas)  
- CacheFly  
- CDN77  
- CDNetworks  
- Cloudflare  
- Cloudflare China Network  
- Fastly  
- GCP Anycast  
- jsDelivr  
- Melbicom / SwiftyCDN  
- Medianova  
- NetEase  
- OVH CDN  
- QUANTIL  
- Tencent EdgeOne  
- Virtuozzo CDN  
- Zenlayer  

*(List may vary slightly depending on version)*

---

## How It Works

The page performs **parallel HTTP requests** to CDN test endpoints and extracts POP / node identifiers from:

- Response headers (e.g. `x-served-by`, `x-amz-cf-pop`, `via`, etc.)
- CDN trace endpoints (e.g. Cloudflare `colo=`)
- Text-based edge endpoints (Apple Edge)

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
- IPv4 vs IPv6 path is observational only (not forced)  
- Results depend on your current network route / ISP / VPN  

---

## Privacy

- No tracking  
- No analytics  
- No data leaves your browser (except normal CDN test requests)  
- Public IP lookup uses `https://ipinfo.io`  

---

## Roadmap / Ideas

- Retry + timeout per CDN  
- POP → City mapping  
- Latency measurement  
- JSON / CSV export  
- Compare runs  
- IPv4 vs IPv6 detection panel  
- Cache HIT / MISS detection  
- Anycast vs Unicast hints  
