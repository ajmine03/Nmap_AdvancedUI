# Nmap TUI — Modern Terminal Interface for Nmap

<p align="center">
  <img src="https://img.shields.io/badge/version-1.0.0-cyan?style=for-the-badge" alt="Version 1.0.0" />
  <img src="https://img.shields.io/badge/python-3.8+-blue?style=for-the-badge&logo=python" alt="Python 3.8+" />
  <img src="https://img.shields.io/badge/license-GPLv2-green?style=for-the-badge" alt="License" />
  <img src="https://img.shields.io/badge/author-ajmine-yellow?style=for-the-badge" alt="Author" />
  <img src="https://img.shields.io/badge/status-stay%20ethical-red?style=for-the-badge" alt="Ethical" />
</p>

A modern, interactive, and fully responsive **Terminal User Interface (TUI)** wrapper built for **Nmap**. It delivers a high-productivity dashboard experience right in your terminal, with clickable panels, real-time command construction, live scan streaming with braille spinner feedback, heuristic severity tagging for open ports, and complete mouse + keyboard controls.

> **"v1.0 By ajmine stay ethical"**

---

## 📸 Interface Preview
<img width="1474" height="966" alt="image" src="https://github.com/user-attachments/assets/cf3f327e-ca41-49ec-9d59-254bde99640f" />



--

## ⚡ Features

- **Interactive 3-Panel Dashboard**:
  - **Scan Profiles (Left)**: Instant profile selection via number keys (`1`-`9`), arrow keys, or mouse click.
  - **Target Information (Top Right)**: Editable target input box supporting hostnames, IPv4, IPv6, and CIDR ranges. Includes quick-click helper targets (`192.168.1.1`, `scanme.nmap.org`, `10.0.0.0/24`).
  - **Live Command Preview**: Displays the exact `nmap` shell command in real-time as you tweak targets and options.
  - **Live Streaming Output (Bottom)**: Live scan streaming directly from the Nmap subprocess with real-time Braille spinner animation (`⠋`, `⠙`, `⠹`, `⠸`, etc.) and an elapsed timer.

- **🖱️ Full Mouse & Keyboard Interactivity**:
  - Click any scan profile to switch mode.
  - Click the Target input box to focus and place the cursor.
  - Click suggestion chips to instantly populate target addresses.
  - Click `[ ▶ EXECUTE SCAN ]` to launch or `[ ■ CANCEL SCAN ]` to abort.
  - Scroll through output using the mouse wheel or `j`/`k`/PageUp/PageDown keys.

- **🛡️ Risk & Severity Tagging**:
  - Automatically enriches open ports and services with color-coded risk indicators:
    - `[CRITICAL]` (Red bold)
    - `[HIGH]` (Red)
    - `[MEDIUM]` (Yellow)
    - `[LOW]` (Cyan)
    - `[INFO]` (Blue)
  - Intelligently parses NSE script outputs (e.g. `--script vuln` CVE findings) and applies known port-risk heuristics for common exposure points (SMB 445, Telnet 23, RDP 3389, Database ports, etc.).
  - Displays a color legend and vulnerability summary count at the footer of the scan report.

- **⚠️ Privilege Detection**:
  - Detects profiles requiring raw socket permissions (`-O`, `-sU`, `-A`).
  - Displays a `⚠ sudo required` warning tag when running as an unprivileged user, and suggests running `sudo ./nmap-tui` for accurate fingerprinting.

- **✨ Clean, Ghost-Free Terminal Display**:
  - Renders inside the terminal's **Alternate Screen Buffer** (`\033[?1049h`), ensuring zero ghosting or bleed-through of previous terminal logs.
  - Dynamic responsive auto-resizing handling terminal resize events (`SIGWINCH`).

- **🔧 Zero Heavy GUI Dependencies**:
  - Built entirely using Python's standard `curses` library and standard libraries.
  - No bloated web frameworks, electron, or third-party TUI frameworks required.

---

## 🎯 Pre-configured Scan Profiles

| # | Profile Name | Nmap Arguments | Description | Privileges |
|---|--------------|----------------|-------------|:----------:|
| **1** | Quick Scan | `-T4 -F` | Fast scan covering top 100 popular ports | User |
| **2** | Full Port Scan | `-p 1-65535` | Complete sweep of all 65,535 TCP ports | User |
| **3** | OS Detection | `-O` | TCP/IP stack fingerprinting for OS identification | **Root / Sudo** |
| **4** | Service Version | `-sV` | Probes open ports to determine service/version info | User |
| **5** | Aggressive Scan | `-A` | OS detection, version scanning, script scanning, traceroute | **Root / Sudo** |
| **6** | Ping Only | `-sn` | Host discovery ping sweep without port scanning | User |
| **7** | Vulnerability Scan | `--script vuln` | Checks target against known NSE vulnerability scripts | User |
| **8** | UDP Scan | `-sU` | Scans common UDP services (DNS, SNMP, DHCP, etc.) | **Root / Sudo** |
| **9** | Fast Scan | `-F` | Fast scan of top 100 ports | User |

---

## 🚀 Getting Started

### Prerequisites

- **Linux / macOS / BSD** (POSIX terminal with ANSI support)
- **Python 3.8+**
- **Nmap** installed (`sudo apt install nmap` / `brew install nmap`)

### Quick Start

1. Clone the repository:
   ```bash
   git clone https://github.com/ajmine/nmap-master.git
   cd nmap-master
   ```

2. Make the launcher executable:
   ```bash
   chmod +x ./nmap-tui
   ```

3. Launch the TUI:
   ```bash
   ./nmap-tui
   ```

> [!TIP]
> For scans requiring raw socket access (OS Detection, UDP, Aggressive Scan), run with sudo:
> ```bash
> sudo ./nmap-tui
> ```

---

## ⌨️ Controls & Keybindings

| Key / Action | Context | Description |
|---|---|---|
| **Mouse Click** | Anywhere | Focus target, select profiles, click buttons, set cursor |
| **Mouse Wheel** | Output Panel | Scroll up / down through scan history and output |
| **Tab** or **t** | Global | Focus / unfocus the Target input box |
| **1 – 9** | Global | Select scan profile 1 through 9 directly |
| **Enter** or **e** | Global | Execute scan on the current target |
| **Esc** or **c** | Target / Scan | Unfocus target edit / Cancel running scan |
| **↑ / ↓** or **k / j** | Output / Profile | Navigate profiles or scroll scan output line-by-line |
| **PgUp / PgDn** | Output Panel | Scroll output 10 lines up or down |
| **Ctrl+L** | Global | Force complete screen redraw and repaint |
| **q** or **Ctrl+C** | Global | Quit the TUI |

---

## 💻 CLI & Headless Usage

The `nmap-tui` binary can also be launched directly with target overrides:

```bash
# Launch TUI with pre-filled target
./nmap-tui --target 192.168.1.1

# Launch TUI with target and profile pre-selected (e.g. Profile 4: Service Version)
./nmap-tui --target 10.10.10.10 --profile 4

# View CLI options
./nmap-tui --help
```

---

## 🧪 Testing

The codebase includes an extensive test suite verifying all modules, curses rendering, mouse coordinate hit-testing, severity heuristics, command generation, and execution pipelines.

### Run Unit Tests
```bash
python3 zenmap/test/test_tui.py
# or
python3 -m unittest zenmap/test/test_tui.py
```
*(Runs 29/29 unit tests covering all TUI components)*

### Run Automated System Check
```bash
chmod +x ./test_all.sh
./test_all.sh --skip-sudo
```
*(Runs end-to-end headless scan verifications, profile builders, and output checks)*

---

## 📂 Architecture

```text
nmap-master/
├── nmap-tui                    # Executable TUI launcher script
├── test_all.sh                 # Full system & integration test script
└── zenmap/
    └── zenmapCore/
        └── tui/
            ├── __init__.py     # Module initialization
            ├── app.py          # Main application controller & event loop
            ├── view.py         # NCurses dashboard layout & rendering engine
            ├── builder.py      # Nmap command construction & flag generator
            ├── runner.py       # Async background scan runner with streaming
            ├── profiles.py     # Scan profiles definitions & configurations
            ├── severity.py     # Heuristic vulnerability & risk tagger
            └── history.py      # Output buffer & scan history manager
```

---

## ⚖️ Legal & Ethical Disclaimer

This tool is designed for educational, defensive security auditing, and authorized system administration purposes only. Scanning targets without prior mutual consent is illegal in most jurisdictions. The authors and contributors accept no liability for misuse of this software.

**Stay Ethical. Scan Responsibly.**

---

## 👤 Author

Developed by **[ajmine](https://github.com/ajmine03)**.
