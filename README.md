# RedSocket
A tool that automates Nmap scans and saves results to a file. More features coming soon
# RedSocket v3.0

**RedSocket** is an interactive command‑line wrapper for Nmap and Masscan designed for fast, practical network reconnaissance with automatic logging, smart output highlighting, and zero‑friction scanning.

---

## Why RedSocket?

Running raw `nmap` commands is powerful but slow in real‑world situations where speed and clarity matter. RedSocket provides:

* Prebuilt scan profiles for common scenarios
* Automatic result saving to Desktop
* Real‑time highlighting of critical services (SSH, RDP, FTP, etc.)
* Session logging for command history
* Built‑in auto‑update system

RedSocket is built for:

* Pentesters
* CTF players
* Lab environments
* Anyone who wants faster recon without memorizing flags

---

## Features

### Scan Profiles

* **Quick TCP Scan** – Fast scan of top ports with service detection
* **Full TCP Scan** – Full 1‑65535 port scan
* **UDP Scan** – Top UDP ports
* **Vulnerability Scan** – Uses Nmap NSE scripts
* **Masscan Scan** – High‑speed scanning with automatic sudo detection

### Custom Command Mode

For advanced users, RedSocket allows full manual Nmap command entry while still providing:

* Optional automatic result saving
* Session logging
* Real‑time terminal output

---

## Critical Service Detection

During scans, RedSocket automatically highlights dangerous or sensitive services in red, for example:

```
[!] CRITICAL SERVICE → SSH (22)
[!] CRITICAL SERVICE → RDP (3389)
```

This allows faster decision‑making during reconnaissance.

---

## Automatic Result Saving

After each scan, you can choose:

* Save results in normal text format (`-oN`)
* Save in all formats (`-oA`)
* Or skip saving

All files are stored on the Desktop for quick access.

---

## Session Logging

All executed commands are stored in:

```
redsocket_session.log
```

This allows you to:

* Reproduce previous scans
* Keep audit history
* Debug scan strategies

---

## Auto Update System

RedSocket can check for new versions directly from GitHub and update itself automatically.

```
Menu → Check for updates
```

---

## Requirements

* Python 3.8+
* Nmap installed and in PATH
* Masscan (optional but recommended)

### Install dependencies (Linux)

```
sudo apt install nmap masscan
```

---

## Installation

```
git clone https://github.com/yourusername/redsocket.git
cd redsocket
python3 redsocket.py
```

---

## Example Workflow

```
python3 redsocket.py
1
192.168.1.10
```

Output:

```
[+] Running: nmap -F -sV 192.168.1.10
[!] CRITICAL SERVICE → SSH (22)
[✔] Scan completed
Results saved to Desktop
```

---

## Project Goals

RedSocket is not meant to replace Nmap. It is designed to make Nmap faster to use in real‑world scenarios where:

* time is limited
* commands are easy to mistype
* results need to be saved automatically

---

## License

MIT License

---

## Disclaimer

Use this tool only on systems you own or have permission to test. Unauthorized scanning may be illegal in your jurisdiction.
