# ISO/OSI and TCP/IP
These models describe how data is transmitted between two devices in a network by defining different layers that handle communication, from sending a request to receiving a response.

### ISO/OSI Model vs. TCP/IP Model  

1. OSI Model (Open Systems Interconnection)  
- A theoretical model developed by ISO to standardize network communication.  
- It has 7 layers, each with a specific function.  
- Mainly used for understanding networking concepts, not directly implemented.  

2. TCP/IP Model (Transmission Control Protocol/Internet Protocol)  
- A practical model, used in real-world networking (including the Internet).  
- It has 4 layers, mapping to OSI layers.  
- Used by modern networking devices and the Internet.  

| Layer | OSI Model (Theoretical) | TCP/IP Model (Practical) | Key Protocols & Examples |  
|-------|-------------------------|--------------------------|--------------------------|  
| 7 | Application | Application | HTTP, HTTPS, FTP, DNS, SSH, SMTP |  
| 6 | Presentation | Merged into Application | SSL/TLS, Encryption, Compression |  
| 5 | Session | Merged into Application | Session Establishment (e.g., TLS handshake) |  
| 4 | Transport | Transport | TCP, UDP (Port Communication) |  
| 3 | Network | Internet | IP, ICMP, ARP, Routing |  
| 2 | Data Link | Network Interface | Ethernet, MAC, VLAN, ARP |  
| 1 | Physical | Network Interface | Cables, Switches, Wi-Fi, NIC |  

### Key Differences  
- OSI Model is a conceptual framework; TCP/IP Model is a practical implementation.  
- OSI has 7 layers, while TCP/IP has 4 layers.  
- TCP/IP merges Application, Presentation, and Session layers into a single Application Layer.  
- The Network Layer in OSI is called the Internet Layer in TCP/IP.  

---

## What Happens When You Type www.google.com in a Browser?  

### Step-by-Step Breakdown (Mapping with OSI & TCP/IP Models)  

Step 1: DNS Resolution (Application Layer – OSI Layer 7 & TCP/IP Layer 4)  
- The browser checks local cache (browser, OS, router).  
- If not found, it sends a DNS request to resolve www.google.com into an IP address.  
- The DNS server responds with Google's IP address (e.g., 142.250.190.78).  

Step 2: Establishing a TCP Connection (Transport Layer – OSI Layer 4 & TCP/IP Layer 3)  
- The browser establishes a TCP connection to Google's server.  
- Uses TCP 3-way handshake (SYN → SYN-ACK → ACK) on port 443 (HTTPS).  

Step 3: Sending an HTTP Request (Application Layer – OSI Layer 7 & TCP/IP Layer 4)  
- The browser sends an HTTPS request (GET www.google.com).  
- The request is encrypted using SSL/TLS.  

Step 4: Data Transmission (Network & Data Link Layers – OSI Layer 3 & 2, TCP/IP Layer 2)  
- Data is packetized and routed to Google’s nearest server.  
- IP packets travel through multiple routers using BGP (Border Gateway Protocol).  

Step 5: Google's Server Processes the Request  
- Google’s web server receives the request, fetches the webpage, and sends a response.  

Step 6: Receiving and Rendering Webpage  
- The browser receives the HTML, CSS, JavaScript content.  
- It renders the webpage and displays it.  

---

## Summary of How OSI & TCP/IP Fit In  

| Step | Process | OSI Layer | TCP/IP Layer |  
|------|---------|-----------|--------------|  
| 1 | DNS Query (www.google.com → IP) | Application (7) | Application (4) |  
| 2 | TCP Handshake (SYN → SYN-ACK → ACK) | Transport (4) | Transport (3) |  
| 3 | HTTPS Request | Application (7) | Application (4) |  
| 4 | Data Packets Routed via Internet | Network (3) | Internet (2) |  
| 5 | Google’s Server Processes Request | Application (7) | Application (4) |  
| 6 | Browser Renders Webpage | Presentation & Application (6 & 7) | Application (4) |  

---

## Conclusion  
- OSI is a theoretical model; TCP/IP is a real-world model used for networking.  
- When you visit www.google.com, multiple network layers work together to resolve the domain, establish a connection, send/receive data, and display the webpage.