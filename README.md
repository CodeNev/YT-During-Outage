# YT During Outage

**A Professional Local Gateway & HAR Analysis Tool for YouTube Traffic**

[![Python Version](https://img.shields.io/badge/python-3.6%2B-blue.svg)](https://www.python.org/downloads/)
[![Flask](https://img.shields.io/badge/flask-2.0%2B-green.svg)](https://flask.palletsprojects.com/)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![GitHub Repo](https://img.shields.io/badge/GitHub-Repo-181717?logo=github)](https://github.com/CodeNev/YT-During-Outage)

---

## 📖 Overview

**YT During Outage** is a production‑grade, single‑file Python application that empowers you to **analyze, clean, and replay** YouTube network traffic captured in HAR (HTTP Archive) files. It acts as a **local HTTP gateway** that can serve previously recorded YouTube resources (videos, thumbnails, fonts, API responses) when live internet access is limited or unavailable.

Unlike conventional proxies or VPNs, this tool does **not** bypass network restrictions by itself—it simply replays what has already been recorded. It is designed for **privacy‑conscious** users who want to inspect, sanitize, and safely reuse captured YouTube traffic for offline study, testing, or controlled viewing.

---

## ✨ Key Features

- **📥 HAR Import & Analysis**  
  Drag‑and‑drop or upload `.har` files from Chrome DevTools. Automatically parses and validates the HAR structure.

- **🧹 Privacy‑Focused Cleaning**  
  Remove cookies, authorization headers, Set‑Cookie, and sensitive query parameters. Customize the list of headers/params to strip.

- **🎬 Video Capability Estimation**  
  Detects whether the HAR contains enough data to play **Shorts** or **long‑form videos**, and estimates the **total playback duration** based on the cached video segments.

- **🌐 Local HTTP Gateway**  
  Start a local server that intercepts browser requests and serves **cached responses** from the HAR file. Works in **YouTube‑only mode** (restricts to YouTube‑related domains) with custom allowlist support.

- **📊 Interactive Dashboard**  
  Real‑time statistics, domain‑category breakdown, request table with search/filter/pagination, and detailed request/response inspection.

- **🖌️ Modern UI with RTL Support**  
  Full Persian (Farsi) interface with right‑to‑left layout, dark/light themes, smooth animations, and offline‑ready Persian fonts (IRANSans, Vazir).

- **⚙️ Advanced Settings**  
  Configure port, upload size, cache, YouTube‑only mode, custom domains, header/parameter blacklists, auto‑browser open, and more.

- **🔒 Security First**  
  Sensitive data detection, automatic masking in logs, no credential theft, local‑only binding (configurable), and strict file/path sanitization.

- **📦 Zero‑External‑Dependency Installation**  
  Auto‑installs Flask (with fallback to regional PyPI mirrors) on first run—no manual `pip install` required.

---

## 🏗️ Architecture

The entire application is contained in a **single Python file** (`youtube_har_gateway.py`). It follows a modular, class‑based design:

```
┌─────────────────────────────────────────────────────────────┐
│                    YT During Outage                        │
├─────────────────────────────────────────────────────────────┤
│  ┌───────────────┐  ┌──────────────┐  ┌───────────────┐  │
│  │ Dependency    │  │   Flask      │  │   Embedded    │  │
│  │ Manager       │──│   Backend    │──│   HTML/CSS/JS │  │
│  │ (auto‑install)│  │   (Routes)   │  │   (Single     │  │
│  └───────────────┘  └──────────────┘  │    Template)  │  │
│                                         └───────────────┘  │
│  ┌───────────────┐  ┌──────────────┐  ┌───────────────┐  │
│  │ HAR Parser    │  │ HAR Analyzer │  │ HAR Cleaner   │  │
│  │ & Validator   │──│ (stats, cat) │──│ (privacy      │  │
│  └───────────────┘  └──────────────┘  │  filtering)   │  │
│                                         └───────────────┘  │
│  ┌───────────────┐  ┌──────────────┐  ┌───────────────┐  │
│  │ Cache Manager │  │ Gateway      │  │ Config        │  │
│  │ (disk‑based)  │──│ Manager      │──│ Manager       │  │
│  └───────────────┘  └──────────────┘  └───────────────┘  │
│  ┌───────────────┐  ┌──────────────┐  ┌───────────────┐  │
│  │ Security      │  │ Logging      │  │ Session State │  │
│  │ Utilities     │  │ System       │  │ (global app)  │  │
│  └───────────────┘  └──────────────┘  └───────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

### Core Components

| Component | Responsibility |
|-----------|----------------|
| **DependencyManager** | Checks for Flask, installs it with fallback mirrors if missing. |
| **HARParser** | Reads and validates HAR files; ensures `log.entries` structure. |
| **HARAnalyzer** | Extracts stats, categorizes domains (YouTube, googlevideo, images, fonts, API, other), detects sensitive data, and identifies video/Shorts segments. |
| **HARCleaner** | Strips cookies, auth headers, Set‑Cookie, and query parameters based on user‑defined rules. |
| **CacheManager** | Stores response bodies and headers on disk for replay; indexed by URL hash. |
| **GatewayManager** | Manages the local HTTP gateway lifecycle (start/stop), enforces YouTube‑only mode, and handles cache lookups. |
| **SecurityManager** | Sanitizes filenames, prevents path traversal, and validates uploads. |
| **ConfigManager** | Loads/saves settings from `runtime/config.json` with sensible defaults. |
| **Flask Routes** | REST API endpoints for upload, analysis, cleaning, gateway control, and settings. |

---

## 🚀 Installation & Running

### Prerequisites
- **Python 3.6+** (3.8+ recommended)
- No other dependencies—Flask is auto‑installed.

### Quick Start

1. **Clone the repository**
   ```bash
   git clone https://github.com/CodeNev/YT-During-Outage.git
   cd YT-During-Outage
   ```

2. **Run the application**
   ```bash
   python youtube_har_gateway.py
   ```

   On first run, the script will:
   - Check Python version.
   - Detect missing Flask.
   - Attempt installation from PyPI (with fallback to regional mirrors if needed).
   - Create runtime directories (`runtime/uploads`, `runtime/cleaned`, `runtime/cache`, `runtime/logs`).
   - Start the Flask development server on `0.0.0.0:8080` (bound to all network interfaces).
   - Automatically open `http://127.0.0.1:8080` in your default browser.

3. **Access the Web UI**
   - Use any device on your local network to connect to `<your‑IP>:8080`.
   - The interface is fully responsive and supports both desktop and mobile.

---

## 🧭 How to Use

### Step 1: Capture a HAR File
1. Open **Chrome** and navigate to **YouTube**.
2. Open **DevTools** (`F12` or `Ctrl+Shift+I` on Windows; `Cmd+Option+I` on macOS).
3. Go to the **Network** tab.
4. Enable **Preserve log** and **Disable cache**.
5. Reload the page and play the video(s) you want to record.
6. Click the **Export** button (down arrow) and save the `.har` file.

### Step 2: Import & Analyze
- On the **Dashboard**, drag‑and‑drop or select your `.har` file.
- The app parses the file and displays statistics:
  - Total requests, YouTube count, googlevideo count, image/font/API/other breakdown.
  - **Video capability estimation**: whether Shorts/long videos are playable and approximate duration.
- Use the **request table** to search, filter by domain/status/type, and view details.

### Step 3: Clean Sensitive Data (Optional)
- Switch to the **Clean** tab.
- Choose which elements to remove:
  - Request cookies (`Cookie`)
  - Authentication headers (`Authorization`, `Proxy‑Authorization`)
  - Response cookies (`Set‑Cookie`)
  - Specific query parameters (custom list)
- Apply **domain filtering** (keep only YouTube‑related domains or custom list).
- Click **Preview** to see what will be removed; then **Generate** to download a sanitized HAR file.

### Step 4: Start the Local Gateway
- After importing, click **START** on the Dashboard.
- The gateway runs on `0.0.0.0:8080` (or your configured port).
- Configure your browser to use `http://<your‑IP>:8080` as a proxy (or simply open `http://127.0.0.1:8080` to access the gateway’s catch‑all route).
- Requests to YouTube‑related domains will be served from the **cache** if a matching response exists; otherwise, they are blocked (in YouTube‑only mode) or return a 404.

### Step 5: Monitor & Stop
- The dashboard shows real‑time gateway status (Running/Stopped) with animated indicators.
- Click **STOP** to halt the gateway.

---

## ⚙️ Advanced Settings

Access the **Settings** modal from the sidebar to customize:

| Setting | Description |
|---------|-------------|
| **Gateway Port** | Port on which the gateway listens (default: `8080`). |
| **Max Upload Size** | Maximum HAR file size in MB (default: `100`). |
| **Enable Cache** | Toggle disk‑based caching of responses. |
| **YouTube‑Only Mode** | Restrict proxying to YouTube‑related domains only. |
| **Custom Allowed Domains** | Extra domains to allow when YouTube‑only mode is enabled. |
| **Auto‑open Browser** | Automatically launch the browser on startup. |
| **Remove Headers** | List of HTTP headers to strip during cleaning. |
| **Remove Query Params** | List of URL parameters to strip during cleaning. |
| **Enable Advanced Filtering** | Enable additional filtering options (e.g., domain filter in Clean tab). |

All settings are persisted in `runtime/config.json`.

---

## 🧪 Technical Limitations

- **No Internet Bypass**  
  This tool does not create new network connectivity. It only replays previously captured responses. If a resource was not recorded in the HAR, it cannot be served—even with the gateway running.

- **Video Playback**  
  YouTube videos are split into multiple segments (typically 5‑10 seconds each). The HAR must contain **all** segments for smooth playback. The app estimates playability based on total video data size, but actual playback may still be incomplete if segments are missing.

- **Proxy vs. Gateway**  
  The local gateway is **not** a full HTTP proxy. It does not forward live requests; it only responds from cache. For live browsing, you still need an active internet connection.

- **Authentication & Sessions**  
  HAR files may contain login cookies or tokens. The app **never** uses these to impersonate users or perform live authenticated actions. Cleaning removes them to protect your privacy.

- **Single‑File Design**  
  Everything is embedded in one Python file. While this simplifies deployment, it also means that the HTML/CSS/JS are not separately editable without modifying the source.

---

## 🗂️ Project Structure

```
YT-During-Outage/
├── youtube_har_gateway.py      # Single monolithic file containing:
│   ├── Dependency Manager
│   ├── Flask Application
│   ├── HAR Parser/Analyzer/Cleaner
│   ├── Cache & Gateway Managers
│   ├── Embedded HTML Template (with CSS/JS)
│   └── Startup Logic
├── runtime/                    # Created on first run
│   ├── uploads/                # Temporary uploaded HAR files
│   ├── cleaned/                # Cleaned HAR downloads
│   ├── cache/                  # Cached response payloads
│   ├── logs/                   # Application logs
│   └── config.json             # Persistent settings
└── README.md                   # This file
```

---

## 🛡️ Security & Privacy

- **Local‑Only by Default** – The server binds to `0.0.0.0` (all interfaces) but you can restrict it to `127.0.0.1` in settings.
- **Filename Sanitization** – All uploaded filenames are hashed and timestamped; original names are never used in filesystem operations.
- **Path Traversal Protection** – All file accesses are resolved and checked against base directories.
- **Sensitive Data Masking** – Logs never contain raw cookies, tokens, or authorization headers. The UI masks values when displayed.
- **No Credential Theft** – The application never extracts or sends authentication tokens to external servers.
- **XSS‑Safe Rendering** – All user‑supplied content is JSON‑escaped before being rendered in the UI.

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository.
2. Create a feature branch (`git checkout -b feature/amazing-feature`).
3. Commit your changes (`git commit -m 'Add some amazing feature'`).
4. Push to the branch (`git push origin feature/amazing-feature`).
5. Open a Pull Request.

### Development Notes
- The entire app is in one file—please maintain that structure.
- Keep the code PEP 8 compliant and add docstrings for new classes/methods.
- If adding dependencies, update the `DependencyManager` list and rationale.

---

## 📄 License

This project is licensed under the **MIT License** – see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- Built with [Flask](https://flask.palletsprojects.com/) – the lightweight Python web framework.
- Inspired by the need to analyze and preserve YouTube traffic during network disruptions.
- Special thanks to the open‑source community for providing the tools and knowledge that made this possible.

---

## 📬 Contact

**Maintainer:** CodeNev  
**GitHub:** [@CodeNev](https://github.com/CodeNev)  
**Repository:** [https://github.com/CodeNev/YT-During-Outage](https://github.com/CodeNev/YT-During-Outage)

---

*Stay connected, even when the network isn’t.*  
**YT During Outage** – Your offline YouTube companion. 🎬
