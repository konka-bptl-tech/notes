# ISO/OSI Model
# TCP/IP Model
# Protocols
# DNS & DHCP
# IP addressing
# CIDR,Mask
# NAT
# LoadBalancing
# Proxy,Forward Proxy, Reverse Proxy
# SSL/ TLS
# 2way,3way handshake

🔐 Security & Firewalls
- Firewall (Stateful vs Stateless)
- IPSec / VPNs
- Zero Trust Security
- Web Security Basics: CORS, HSTS, CSP

🌍 Advanced Networking
- BGP / OSPF (Routing Protocols)
- IPv6 Concepts
- Multicast, Anycast, Unicast, Broadcast

📡 Observability
- Netstat / Traceroute / Wireshark
- Ping / ICMP
- Connection timeouts, retries, TCP states

---

## 🧠 **1. Interface & IP Address Management**

| Command | Description |
|--------|-------------|
| `ip a` or `ip addr` | Show all IP addresses and interfaces |
| `ip link` | Show all interfaces (up/down, MACs) |
| `ip link set eth0 up/down` | Enable/disable interface |
| `ip addr add 192.168.1.10/24 dev eth0` | Add IP to an interface |
| `ifconfig` | Legacy command (use `ip` now) |
| `ip route` | View routing table |
| `ip r add default via 192.168.1.1` | Add a default gateway |

---

## 🌍 **2. DNS & Name Resolution**

| Command | Description |
|--------|-------------|
| `nslookup google.com` | Query DNS servers (basic lookup) |
| `dig google.com` | Advanced DNS lookup |
| `host google.com` | Simple DNS lookup |
| `cat /etc/resolv.conf` | Check DNS resolver config |
| `systemd-resolve --status` | DNS info per interface |

---

## 🔁 **3. Connectivity & Troubleshooting**

| Command | Description |
|--------|-------------|
| `ping 8.8.8.8` | Test network reachability |
| `traceroute google.com` | Trace network path to host |
| `mtr google.com` | Dynamic traceroute (real-time stats) |
| `curl -I https://example.com` | Test HTTP response headers |
| `wget --spider http://site.com` | Check if a URL is accessible |
| `nc -zv host port` | Check if a port is open (netcat) |

---

## 🔍 **4. Port Listening & Services**

| Command | Description |
|--------|-------------|
| `ss -tuln` | Show listening TCP/UDP ports |
| `ss -plant` | Show listening + processes |
| `netstat -tuln` | Legacy version of `ss` |
| `lsof -i :80` | Show process using port 80 |
| `systemctl status <service>` | Check status of a service |

---

## 🛜 **5. Network Stats & Bandwidth**

| Command | Description |
|--------|-------------|
| `ifstat` | Real-time bandwidth per interface |
| `vnstat` | Bandwidth usage logs (install needed) |
| `iftop` | Real-time bandwidth per connection |
| `ip -s link` | Interface traffic stats |
| `nload` | Real-time in/out graphs |

---

## 🔐 **6. Firewall / Packet Filtering (iptables or nftables)**

| Command | Description |
|--------|-------------|
| `iptables -L` | List firewall rules |
| `iptables -A INPUT -p tcp --dport 80 -j ACCEPT` | Allow port 80 |
| `nft list ruleset` | If using `nftables` |
| `ufw status` | Ubuntu firewall tool |
| `firewalld` | Used in CentOS/RHEL (with `firewall-cmd`) |

---

## 🧰 **7. Advanced Tools (for pros)**

| Command | Description |
|--------|-------------|
| `tcpdump -i eth0` | Capture packets |
| `wireshark` | GUI packet sniffer |
| `ethtool eth0` | NIC speed, duplex, etc. |
| `nmcli` | Network Manager CLI tool |
| `arp -a` | View ARP cache |
| `ip neigh` | View neighbor table (ARP + NDP) |
| `route -n` | Show routing table (legacy) |

---

## 🧪 **8. Testing & Temporary Changes**

| Command | Description |
|--------|-------------|
| `tc` | Simulate latency, packet loss |
| `iptables` | Drop packets for testing |
| `ip rule add` | Policy routing |
| `ip netns` | Network namespaces (for simulating multiple nodes) |

---
