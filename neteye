#!/usr/bin/env python3
"""
NetEye - Advanced Port Scanner v2.0
A user-friendly port and DNS scanning tool with stealth mode.

Usage:
    python3 neteye.py <host> <start-end> [options]
    python3 neteye.py <host> --dns
    
Examples:
    python3 neteye.py 127.0.0.1 20-1000
    python3 neteye.py example.com 1-1024 --stealth
    python3 neteye.py google.com --dns
"""

import socket
import sys
import time
import argparse
from typing import List, Tuple

# Common ports for quick scan
COMMON_PORTS = [21, 22, 23, 25, 53, 80, 110, 143, 443, 445, 3306, 3389, 5432, 8080, 8443]

def banner():
    """Display tool banner."""
    print("""
╔═══════════════════════════════════════╗
║      NetEye Port Scanner v2.0         ║
║      Network Reconnaissance Tool      ║
║      Dev: DezTheJackal                ║
╚═══════════════════════════════════════╝
""")

def resolve_host(host: str) -> Tuple[str, str]:
    """
    Resolve hostname to IP address.
    Returns: (hostname, ip_address)
    """
    try:
        ip = socket.gethostbyname(host)
        return (host, ip)
    except socket.gaierror:
        print(f"[!] Error: Cannot resolve hostname '{host}'")
        sys.exit(1)

def dns_scan(host: str):
    """Perform DNS reconnaissance on target."""
    print(f"\n[+] DNS Reconnaissance for: {host}")
    print("=" * 50)
    
    # Get IP address
    try:
        ip = socket.gethostbyname(host)
        print(f"[+] IPv4 Address: {ip}")
    except socket.gaierror:
        print(f"[-] Could not resolve IPv4 address")
        ip = None
    
    # Reverse DNS lookup
    if ip:
        try:
            hostname = socket.gethostbyaddr(ip)
            print(f"[+] Hostname: {hostname[0]}")
            if hostname[1]:
                print(f"[+] Aliases: {', '.join(hostname[1])}")
        except socket.herror:
            print(f"[-] No reverse DNS record found")
    
    # Get all addresses (IPv4 and IPv6)
    try:
        addr_info = socket.getaddrinfo(host, None)
        ipv6_addrs = set()
        ipv4_addrs = set()
        
        for addr in addr_info:
            if addr[0] == socket.AF_INET6:
                ipv6_addrs.add(addr[4][0])
            elif addr[0] == socket.AF_INET:
                ipv4_addrs.add(addr[4][0])
        
        if ipv6_addrs:
            print(f"[+] IPv6 Addresses: {', '.join(ipv6_addrs)}")
        
        if len(ipv4_addrs) > 1:
            print(f"[+] Additional IPv4: {', '.join(ipv4_addrs - {ip})}")
            
    except socket.gaierror:
        pass
    
    print("=" * 50)

def get_service_name(port: int) -> str:
    """Get common service name for a port."""
    try:
        return socket.getservbyport(port)
    except OSError:
        return "unknown"

def scan_port(target: str, port: int, timeout: float = 1.0, stealth: bool = False) -> bool:
    """
    Scan a single port.
    Returns True if port is open.
    """
    try:
        sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        sock.settimeout(timeout)
        
        # Stealth mode: add random delay
        if stealth:
            time.sleep(0.1)
        
        result = sock.connect_ex((target, port))
        sock.close()
        
        return result == 0
    except socket.error:
        return False
    except Exception:
        return False

def scan_ports(target: str, start_port: int, end_port: int, 
               timeout: float = 1.0, stealth: bool = False, verbose: bool = False) -> List[int]:
    """
    Scan ports from start_port to end_port on target host.
    Returns list of open ports.
    """
    open_ports = []
    total_ports = end_port - start_port + 1
    
    print(f"\n[+] Scanning {total_ports} ports on {target}...")
    if stealth:
        print("[+] Stealth mode enabled (slower but less detectable)")
    print("[+] Press Ctrl+C to stop\n")
    
    try:
        for i, port in enumerate(range(start_port, end_port + 1), 1):
            if verbose and i % 100 == 0:
                print(f"[*] Progress: {i}/{total_ports} ports scanned...", end='\r')
            
            if scan_port(target, port, timeout, stealth):
                service = get_service_name(port)
                open_ports.append(port)
                print(f"[✓] Port {port:5d} OPEN  [{service}]")
        
        if verbose:
            print(f"[*] Progress: {total_ports}/{total_ports} ports scanned...")
            
    except KeyboardInterrupt:
        print("\n\n[!] Scan interrupted by user.")
        print(f"[*] Scanned {i} out of {total_ports} ports")
    
    return open_ports

def quick_scan(target: str, timeout: float = 0.5) -> List[int]:
    """Scan common ports quickly."""
    print(f"\n[+] Quick scan of common ports on {target}...")
    open_ports = []
    
    for port in COMMON_PORTS:
        if scan_port(target, port, timeout, False):
            service = get_service_name(port)
            open_ports.append(port)
            print(f"[✓] Port {port:5d} OPEN  [{service}]")
    
    return open_ports

def main():
    parser = argparse.ArgumentParser(
        description='NetEye - Network port scanner and DNS reconnaissance tool',
        formatter_class=argparse.RawDescriptionHelpFormatter,
        epilog="""
Examples:
  python3 neteye.py 192.168.1.1 1-1000
  python3 neteye.py example.com 20-100 --stealth
  python3 neteye.py example.com --dns
  python3 neteye.py 10.0.0.1 --quick
  python3 neteye.py scanme.nmap.org 1-1024 -t 0.5 -v
        """
    )
    
    parser.add_argument('host', help='Target host (IP or domain)')
    parser.add_argument('range', nargs='?', help='Port range (e.g., 1-1000)')
    parser.add_argument('--dns', action='store_true', help='Perform DNS reconnaissance only')
    parser.add_argument('--quick', action='store_true', help='Quick scan of common ports')
    parser.add_argument('--stealth', '-s', action='store_true', help='Enable stealth mode (slower)')
    parser.add_argument('--timeout', '-t', type=float, default=1.0, help='Connection timeout (default: 1.0s)')
    parser.add_argument('--verbose', '-v', action='store_true', help='Verbose output')
    
    args = parser.parse_args()
    
    banner()
    
    # Resolve hostname
    hostname, ip = resolve_host(args.host)
    if hostname != ip:
        print(f"[+] Target: {hostname} ({ip})")
    else:
        print(f"[+] Target: {ip}")
    
    # DNS scan mode
    if args.dns:
        dns_scan(args.host)
        return
    
    # Quick scan mode
    if args.quick:
        open_ports = quick_scan(ip, args.timeout)
    else:
        # Regular port scan
        if not args.range:
            print("[!] Error: Port range required (e.g., 1-1000)")
            print("    Use --quick for common ports or --dns for DNS scan")
            sys.exit(1)
        
        try:
            start, end = [int(x) for x in args.range.split("-")]
            if start < 1 or end > 65535 or start > end:
                raise ValueError
        except ValueError:
            print("[!] Invalid port range. Use format: 1-1000 (ports must be 1-65535)")
            sys.exit(1)
        
        open_ports = scan_ports(ip, start, end, args.timeout, args.stealth, args.verbose)
    
    # Summary
    print("\n" + "=" * 50)
    print(f"[+] Scan Complete!")
    if open_ports:
        print(f"[+] Found {len(open_ports)} open port(s): {open_ports}")
    else:
        print("[+] No open ports found")
    print("=" * 50)

if __name__ == "__main__":
    try:
        main()
    except KeyboardInterrupt:
        print("\n[!] Exiting...")
        sys.exit(0)
