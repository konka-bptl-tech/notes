## 🌐 ISO/OSI vs TCP/IP – **Network Layers Explained**

### **1. ISO/OSI Model (7 Layers)**

A conceptual model to understand **how data flows in a network**.

| Layer | Name         | What It Does                                |
| ----- | ------------ | ------------------------------------------- |
| 7     | Application  | User-facing apps (HTTP, FTP, Email)         |
| 6     | Presentation | Data formatting, encryption (SSL, ASCII)    |
| 5     | Session      | Starts/stops communication (sessions)       |
| 4     | Transport    | Reliable delivery (TCP/UDP, ports)          |
| 3     | Network      | Routing data across networks (IP)           |
| 2     | Data Link    | MAC addresses, switching (Ethernet, frames) |
| 1     | Physical     | Cables, Wi-Fi, actual transmission          |

> **Why**: It helps understand, troubleshoot, and design network communication by isolating responsibilities.

---

### **2. TCP/IP Model (4 Layers)**

The **real-world implementation** model, simpler than OSI.

| Layer | Corresponds to OSI Layers | What It Does                       |
| ----- | ------------------------- | ---------------------------------- |
| 4     | Application (7–5)         | Apps using the network (HTTP, DNS) |
| 3     | Transport (4)             | End-to-end communication (TCP/UDP) |
| 2     | Internet (3)              | Addressing & routing (IP)          |
| 1     | Link (2–1)                | Physical and MAC layer             |

> **Why**: This is the model used in practice (e.g., the Internet), based on real protocols.

---

## 🔀 Reverse Proxy vs Forward Proxy

### **Forward Proxy**

* **Sits in front of clients** (like browsers)
* Forwards requests **from client to the internet**

> **Used for**:

* Anonymity (hide client IP)
* Caching
* Access control (e.g., content filters)

### **Reverse Proxy**

* **Sits in front of servers**
* Forwards requests **from the internet to internal servers**

> **Used for**:

* Load balancing
* SSL termination
* Caching
* Application firewall

> **Why**: Proxies improve **security**, **performance**, and **scalability** in networks.

---

## 🤝 TCP Handshake (3-Way Handshake)

### **What**: The process to establish a **reliable** connection before data exchange using TCP.

### **Steps**:

1. **SYN**: Client → Server → *"Can I connect?"*
2. **SYN-ACK**: Server → Client → *"Yes, you can!"*
3. **ACK**: Client → Server → *"Thanks, I'm ready."*

> **Why**: Ensures both sides are **ready** and agree to the connection settings (e.g., port, sequence numbers).

---

## 🔐 SSL/TLS Basics

### **What**:

* SSL (now deprecated) and TLS are cryptographic protocols that **secure data over the internet**.
* TLS = "Transport Layer Security" (modern standard).

### **What It Does**:

* **Encrypts data** (so attackers can't read it)
* **Authenticates** the server (via certificates)
* **Ensures integrity** (data not altered)

> **Why**: Essential for secure web browsing (HTTPS), secure email, VPNs, etc.

---

## ✂️ SSL Termination

### **What**:

* The process of **decrypting SSL/TLS traffic at a proxy or load balancer** before it reaches the application server.

> Typically done by:

* Reverse proxies (like NGINX, HAProxy)
* Load balancers (like AWS ELB)

### **Why**:

* Offloads the **CPU-intensive decryption** from backend servers
* Enables **traffic inspection**, **load balancing**, and easier **certificate management**

---