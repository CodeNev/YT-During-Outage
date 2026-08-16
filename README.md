
# 🎬 YT During Outage

**A local HAR analysis & lightweight HTTP gateway for YouTube traffic, built as a single-file Python Flask application.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.6+](https://img.shields.io/badge/python-3.6+-blue.svg)](https://www.python.org/downloads/)
[![Flask](https://img.shields.io/badge/Flask-2.0+-green.svg)](https://flask.palletsprojects.com/)

---

## 📖 Overview

**YT During Outage** is a comprehensive, self-contained tool for:

- **Importing** `.har` (HTTP Archive) files captured from Chrome DevTools.
- **Analyzing** YouTube-related network requests and detecting video segments, shorts, long videos, resolutions, and estimated watch time.
- **Cleaning** sensitive data (cookies, authorization headers, query parameters, Set-Cookie) from HAR files with preview and domain filtering.
- **Replaying** cached responses locally via a built‑in HTTP gateway (YouTube‑only mode) to serve previously captured resources when live connectivity is limited.

> ⚠️ **Important:** A HAR file is a **recording** of past network activity; it **does not** create new Internet access or bypass a complete network shutdown. The gateway can only replay resources that are already present in the uploaded HAR and reachable from your local network.

---

## ✨ Features

- **Single‑File Application** – Everything (backend, frontend, CSS, JS, SVG icons) is embedded in `youtube_har_gateway.py`. No external templates, static files, or configuration required.
- **Persian UI** – Full RTL interface with Persian labels, buttons, error messages, tooltips, and comprehensive help documentation.
- **Auto‑Dependency Installation** – Checks for Flask and installs missing packages automatically using official PyPI with fallback mirrors (Liara, Runflare, Abrha).
- **Smart HAR Analysis** – Detects YouTube, googlevideo, ytimg, ggpht, fonts.googleapis.com, and other related domains; categorizes requests and identifies video segments.
- **Video Prediction** – Estimates:
  - Number of Shorts vs. long videos
  - Total video data size
  - Estimated watch time (minutes)
  - Dominant resolution (144p – 4K) and quality tier
- **Advanced Cleaning** – Remove cookies, authorization headers, Set‑Cookie, custom sensitive headers, and query parameters; filter by domain (YouTube‑only, custom allowlist, or keep all).
- **Local HTTP Gateway** – Serves cached responses from the HAR when `START` is triggered; operates in YouTube‑only mode by default (blocks arbitrary domains).
- **Live Dashboard** – Displays request count, YouTube/googlevideo breakdown, bandwidth usage, average response time, sensitive data detection, and font status.
- **Dark / Light Theme** – Persisted via `localStorage`; smooth transition without page reload.
- **Responsive Design** – Works on desktops, tablets, and mobile phones (bottom navigation on small screens).
- **Exportable Report** – Generate a plain‑text summary of all analysis results, including top hosts, HTTP methods, status codes, and video predictions.
- **Quick Clean** – One‑click cleaning with default safe settings, followed by immediate download.

---

## 🚀 Installation & Running

### Prerequisites
- Python 3.6 or higher
- Internet connection (only for first‑run dependency installation and optional font download)

### Steps

1. **Download the single file**  
   Save `youtube_har_gateway.py` to your desired directory.

2. **Run the application**  
   ```bash
   python youtube_har_gateway.py
   ```
   - The script will automatically check for Flask and install it if missing.
   - On the first run, it will also attempt to download the **Vazir Bold** font (from the official GitHub release) to `runtime/fonts/` for offline use. If the download fails, it falls back to Google Fonts and system fonts.

3. **Open the web interface**  
   The server starts on `http://0.0.0.0:8080` by default. Your default browser will open automatically.  
   You can change the port in the **Settings** modal.

4. **Start using**  
   - Upload a `.har` file (drag‑and‑drop or click to select).
   - Wait for the analysis to complete – statistics and video predictions will appear.
   - Use the **START** button to launch the local gateway.
   - Configure your browser to use `http://127.0.0.1:8080` as an HTTP proxy (optional; the gateway also works as a stand‑alone local server).

---

## 🧩 Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Single‑File Flask App                    │
│                   youtube_har_gateway.py                    │
├─────────────────────────────────────────────────────────────┤
│  ┌─────────────┐   ┌─────────────┐   ┌─────────────────┐  │
│  │  HAR Parser  │   │ HAR Analyzer│   │  HAR Cleaner    │  │
│  │  (JSON +     │   │ (Categorize,│   │  (Remove        │  │
│  │   structure) │   │  predict)   │   │   sensitive)    │  │
│  └─────────────┘   └─────────────┘   └─────────────────┘  │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │           Local HTTP Gateway (Caching)              │   │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────────────┐ │   │
│  │  │ Cache    │  │ YouTube  │  │ Block non‑YouTube│ │   │
│  │  │ Manager  │  │ Only     │  │ domains (if ON)  │ │   │
│  │  └──────────┘  └──────────┘  └──────────────────┘ │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │              Configuration & Logging                 │   │
│  │  runtime/config.json + runtime/logs/app.log         │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │      Embedded UI (HTML + CSS + JS + SVG)           │   │
│  │  Rendered with Flask.render_template_string()      │   │
│  └─────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

---

## 🔌 API Endpoints

| Method | Endpoint               | Description |
|--------|------------------------|-------------|
| POST   | `/api/upload`          | Upload a `.har` file (multipart/form-data) |
| GET    | `/api/analyze`         | Retrieve analysis results for the uploaded HAR |
| POST   | `/api/clean`           | Clean the HAR based on provided options (JSON body) |
| POST   | `/api/quick_clean`     | Clean with default safe settings and return the cleaned file |
| GET    | `/api/download/<file>` | Download a cleaned HAR file |
| POST   | `/api/gateway/start`   | Start the local HTTP gateway |
| POST   | `/api/gateway/stop`    | Stop the local HTTP gateway |
| GET    | `/api/gateway/status`  | Get gateway status (running/stopped) |
| GET    | `/api/stats`           | Get current statistics (total, categories, bandwidth, etc.) |
| GET    | `/api/requests`        | Paginated, filterable list of requests (supports `search`, `domain`, `status`, `type`, `page`, `per_page`) |
| GET    | `/api/export_report`   | Generate a plain‑text summary report of the analysis |
| POST   | `/api/settings`        | Update configuration (JSON body) |
| POST   | `/api/clear_session`   | Clear uploaded HAR and analysis state |
| POST   | `/api/cleanup`         | Remove temporary files (uploads, cleaned, cache, logs) |

All JSON responses follow the structure:
```json
{
  "success": true,
  "message": "...",
  "data": { ... }
}
```
Errors return `"success": false` with an explanatory `"error"` field.

---

## 🛠 Configuration

Settings are stored in `runtime/config.json` and can be modified via the UI Settings modal or by editing the file directly. Available options:

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `gateway_host` | string | `0.0.0.0` | Host to bind the gateway server |
| `gateway_port` | integer | `8080` | Port for the gateway |
| `upload_max_size_mb` | integer | `100` | Maximum HAR file size in MB |
| `cache_enabled` | boolean | `true` | Enable response caching from HAR |
| `cache_max_size_mb` | integer | `500` | Max cache size in MB |
| `youtube_only_mode` | boolean | `true` | Block non‑YouTube domains in gateway mode |
| `custom_allowed_domains` | array | `[]` | Additional domains to allow in YouTube‑only mode |
| `auto_open_browser` | boolean | `true` | Automatically open browser on startup |
| `theme` | string | `light` | `light` or `dark` |
| `log_level` | string | `INFO` | Python logging level |
| `remove_headers` | array | `['cookie','authorization','proxy-authorization','set-cookie']` | Headers to remove during cleaning |
| `remove_query_params` | array | `['utm_source','utm_medium','utm_campaign','sid','token','auth']` | Query parameters to strip during cleaning |

---

## 📦 Technology Stack

- **Backend:** Python 3.6+, Flask 2.0+
- **Frontend:** Vanilla JavaScript, HTML5, CSS3 (Glassmorphism, responsive grids)
- **Fonts:** Google Fonts (Vazirmatn) + local fallback (Vazir Bold downloaded to `runtime/fonts/`)
- **Storage:** JSON files (runtime directory for config, cache, uploads, cleaned HARs, logs)
- **No external dependencies beyond Flask** – all other libraries are from the Python standard library.

---

## 📜 License

This project is licensed under the **MIT License** – see the [LICENSE](https://github.com/CodeNev/YT-During-Outage/blob/main/LICENSE) file for details.

```
MIT License

Copyright (c) 2026 CodeNev

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository.
2. Create a feature branch (`git checkout -b feature/your-feature`).
3. Commit your changes (`git commit -m 'Add some feature'`).
4. Push to the branch (`git push origin feature/your-feature`).
5. Open a Pull Request.

For bug reports or feature requests, please use the [GitHub Issues](https://github.com/CodeNev/YT-During-Outage/issues) page.

---

## 📄 Acknowledgements

- **Vazir Font** by Saber Rastikerdar – used under the public domain.
- **Google Fonts** – Vazirmatn as a web‑safe alternative.
- **Flask** – the lightweight WSGI web framework.

---

## 🔗 Links

- **GitHub Repository:** [https://github.com/CodeNev/YT-During-Outage](https://github.com/CodeNev/YT-During-Outage)
- **Report Issues:** [https://github.com/CodeNev/YT-During-Outage/issues](https://github.com/CodeNev/YT-During-Outage/issues)

---

*Built with ❤️ for offline YouTube analysis and educational purposes.*
```
