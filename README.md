# NetEye
scanner.py is a lightweight, beginner-friendly port scanning tool written in pure Python 3. It lets you quickly check a target host for open ports within a given range. No extra libraries, no setup hassle — just clone and run.
_____________________________
Dev: DezTheJackal
_____________________________
# NetEye.py 

A **super simple port scanner and network scanner** written in **pure Python 3**.  
No dependencies, no setup headaches. Just clone and run.

# NetEye 👁️

A **powerful yet simple** port scanning and DNS reconnaissance tool written in **pure Python 3**.  
No external dependencies required — just Python 3.6+ and you're ready to go!

 **Legal Notice:** Only scan systems you own or have explicit written permission to test. Unauthorized port scanning may be illegal in your jurisdiction.

---

##  Features

-  **Port Scanning** - Scan single ports or ranges (1-65535)
-  **Stealth Mode** - Slower scanning to evade detection
-  **DNS Reconnaissance** - Resolve hostnames, reverse DNS, IPv4/IPv6
-  **Quick Scan** - Rapidly check common ports (FTP, SSH, HTTP, etc.)
-  **User-Friendly** - Clear output with service names
-  **Error Handling** - Graceful handling of interrupts and errors
-  **Verbose Mode** - Real-time progress updates
-  **Configurable** - Adjust timeout and scanning speed

---

##  Quick Start

### Installation

```bash
# Clone the repository
git clone https://github.com/DezTheJackal/NetEye.git
cd NetEye

# Make executable (optional)
chmod +x neteye.py
```

### Basic Usage

```bash
# Scan ports 1-1000 on localhost
python3 neteye.py 127.0.0.1 1-1000

# Scan a domain
python3 neteye.py example.com 20-100

# Quick scan of common ports
python3 neteye.py 192.168.1.1 --quick

# DNS reconnaissance
python3 neteye.py google.com --dns
```

---

##  Usage Examples

### Standard Port Scan
```bash
python3 neteye.py 192.168.1.1 1-1000
```
Output:
```
╔═══════════════════════════════════════╗
║      NetEye Port Scanner v2.0         ║
║      Network Reconnaissance Tool      ║
║      Dev: DezTheJackal                ║
╚═══════════════════════════════════════╝

[+] Scanning 1000 ports on 192.168.1.1...
[✓] Port    22 OPEN  [ssh]
[✓] Port    80 OPEN  [http]
[✓] Port   443 OPEN  [https]

[+] Found 3 open port(s): [22, 80, 443]
```

### Stealth Mode Scan
Adds delays between requests to avoid detection:
```bash
python3 neteye.py 10.0.0.1 1-100 --stealth
```

### Quick Scan (Common Ports)
Scans the most common ports (FTP, SSH, HTTP, MySQL, etc.):
```bash
python3 neteye.py scanme.nmap.org --quick
```

### DNS Reconnaissance
```bash
python3 neteye.py example.com --dns
```
Output:
```
[+] DNS Reconnaissance for: example.com
==================================================
[+] IPv4 Address: 93.184.216.34
[+] Hostname: example.com
[+] IPv6 Addresses: 2606:2800:220:1:248:1893:25c8:1946
==================================================
```

### Advanced Options
```bash
# Faster scan with 0.5s timeout
python3 neteye.py 192.168.1.1 1-10000 -t 0.5

# Verbose mode with progress
python3 neteye.py 10.0.0.1 1-5000 --verbose

# Combine stealth + verbose
python3 neteye.py example.com 1-1024 --stealth -v
```

---

##  Command Line Options

```
usage: neteye.py [-h] [--dns] [--quick] [--stealth] [--timeout TIMEOUT] [--verbose] host [range]

positional arguments:
  host                  Target host (IP or domain)
  range                 Port range (e.g., 1-1000)

optional arguments:
  -h, --help            Show help message
  --dns                 Perform DNS reconnaissance only
  --quick               Quick scan of common ports
  --stealth, -s         Enable stealth mode (slower)
  --timeout, -t         Connection timeout in seconds (default: 1.0)
  --verbose, -v         Show verbose output with progress
```

---

##  Common Port Reference

The `--quick` option scans these common ports:

| Port | Service      | Description                    |
|------|--------------|--------------------------------|
| 21   | FTP          | File Transfer Protocol         |
| 22   | SSH          | Secure Shell                   |
| 23   | Telnet       | Telnet                         |
| 25   | SMTP         | Email (Send)                   |
| 53   | DNS          | Domain Name System             |
| 80   | HTTP         | Web Server                     |
| 110  | POP3         | Email (Receive)                |
| 143  | IMAP         | Email (Receive)                |
| 443  | HTTPS        | Secure Web Server              |
| 445  | SMB          | Windows File Sharing           |
| 3306 | MySQL        | MySQL Database                 |
| 3389 | RDP          | Remote Desktop Protocol        |
| 5432 | PostgreSQL   | PostgreSQL Database            |
| 8080 | HTTP-Alt     | Alternative Web Server         |
| 8443 | HTTPS-Alt    | Alternative Secure Web Server  |

---

##  Tips & Best Practices

### Performance Optimization
- **Fast Scan**: Use shorter timeout `-t 0.3` for local networks
- **Thorough Scan**: Use longer timeout `-t 2.0` for unstable connections
- **Stealth Scan**: Enable `--stealth` when scanning external targets

### Scanning Strategy
1. Start with `--quick` to identify common services
2. Use `--dns` to gather reconnaissance data
3. Perform full range scan `1-65535` if needed
4. Use stealth mode for external/sensitive targets

### Avoiding Detection
- Use `--stealth` mode for slower, less obvious scanning
- Scan during off-peak hours
- Use smaller port ranges
- Increase timeout to reduce connection attempts

---

##  Troubleshooting

**"Cannot resolve hostname"**
- Check spelling of domain name
- Verify internet connection
- Try using IP address instead

**"Connection timeout"**
- Increase timeout: `-t 2.0`
- Check firewall settings
- Verify target is reachable: `ping <host>`

**Slow scanning**
- Reduce timeout: `-t 0.5`
- Disable stealth mode
- Scan smaller port ranges

**Permission errors**
- Some systems require root/admin for low ports
- Try scanning ports above 1024
- Run with sudo (Linux/Mac): `sudo python3 neteye.py ...`

---

##  Legal & Ethical Use

This tool is for **authorized security testing and network administration only**.

 **Allowed Uses:**
- Scanning your own systems
- Authorized penetration testing
- Network administration with permission
- Educational purposes on authorized systems

 **Prohibited Uses:**
- Scanning systems without permission
- Unauthorized network reconnaissance
- Malicious activity of any kind

**Remember:** Unauthorized port scanning may violate computer fraud laws in many countries.

---

##  Requirements

- Python 3.6 or higher
- No external dependencies!

---

##  Version History

**v2.0** (Current)
- Rebranded to NetEye
- Added DNS reconnaissance
- Added stealth mode
- Added quick scan mode
- Enhanced user interface
- Better error handling
- Service name detection
- Verbose progress mode

**v1.0**
- Initial release as scanner.py
- Basic port scanning

---

## Author

**DezTheJackal**

---

##  License

This project is provided as-is for educational and authorized security testing purposes.

---

##  Contributing

Contributions are welcome! Feel free to:
- Report bugs
- Suggest features
- Submit pull requests

Visit: https://github.com/DezTheJackal/NetEye

---

##  Show Your Support

If you find NetEye useful, please consider giving it a star on GitHub!

---

##  Resources

- **Documentation**: Full docs available in the repo
- **Issues**: Report bugs on GitHub Issues
- **Community**: Join discussions on GitHub

---

**Keep your network in sight with NetEye! **
