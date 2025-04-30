# Terminology
# **Load Balancing**
- It is the technique of distributing network or application traffic across multiple servers.
- Purpose: maximize throughput, minimize response time, and avoid overloading any single server.
- Can be done at different layers:  
  - **Layer 4 (Transport Layer):** Based on IP address and TCP/UDP ports.  
  - **Layer 7 (Application Layer):** Based on HTTP headers, cookies, URL paths.
---
# **High Availability (HA)**
- The design principle of building systems that minimize downtime and ensure continuous operations.
- Achieved through redundancy at every layer (servers, databases, network devices).
- Requires monitoring, health checks, and automatic failover mechanisms.
- Often expressed in uptime guarantees (e.g., "99.99% availability").
---
# **Fault Tolerance**
- The capability of a system to continue operating properly in the event of the failure of one or more components.
- Implemented by replicating critical components and providing immediate backup functionality without interruption.
- Fault-tolerant systems usually involve hardware redundancy, database replication, and network redundancy.
---
# **Failover**
- A mechanism that automatically switches to a standby or redundant system when the primary system fails.
- Failover can be **active-passive** (only standby server activated upon failure) or **active-active** (both systems active simultaneously).
---
# **AutoScaling**
- The automatic adjustment of the number of compute resources (like servers or pods) based on current demand.
- Helps maintain application performance while optimizing cost.
  ## **Scale Out**
  - Adding more instances/resources when load increases (e.g., adding more servers during peak traffic).
  ## **Scale In**
  - Removing instances/resources when load decreases (e.g., reducing servers during low traffic periods).
---
# Network Concepts
A **Proxy** acts as an intermediary between a client and a server, facilitating requests and responses. There are two common types of proxies: **Forward Proxy** and **Reverse Proxy**.
### 1. **Forward Proxy:**
A **forward proxy** is used by clients (typically users or devices) to connect to a server. The client sends requests to the forward proxy, which then forwards these requests to the target server. The server's response is sent back through the proxy to the client.
#### Use cases for Forward Proxy:
- **Privacy and Security:** It hides the client’s real IP address, making it harder for the server to track the client.
- **Content Filtering:** It can block certain content or websites for users within a network.
- **Bypass Geo-Restrictions:** It helps users access content or websites that are restricted to certain geographical locations.
#### Example:
- A user in a corporate network accesses a website through a forward proxy, which hides their IP and might enforce company-wide internet policies (like blocking certain websites).
---
### 2. **Reverse Proxy:**
A **reverse proxy** is used by servers to handle requests on behalf of another server or multiple servers. The client sends requests to the reverse proxy, which forwards these requests to the appropriate server, and then sends the response back to the client.
#### Use cases for Reverse Proxy:
- **Load Balancing:** It can distribute incoming traffic across multiple servers, balancing the load.
- **Security:** The reverse proxy can act as a shield between the client and the backend servers, preventing direct access to the backend.
- **Caching:** It can cache content, reducing the load on backend servers by serving cached responses.
- **SSL Termination:** It can handle SSL encryption, offloading this task from the backend servers.
#### Example:
- A reverse proxy sits between the client and a set of backend servers hosting a website, managing traffic distribution and adding layers of security.
---
### Key Differences:
- **Forward Proxy:** Client-side proxy. The client makes requests to the proxy, which forwards them to the server.
- **Reverse Proxy:** Server-side proxy. The client makes requests to the reverse proxy, which forwards them to the appropriate server.
---
# **SSL/TLS Basics**
- **SSL** (Secure Sockets Layer) and **TLS** (Transport Layer Security) are **protocols** that **encrypt** data between a client and server.
- They **make sure** that whatever you send (like passwords, credit card numbers) stays **private** and **cannot be intercepted**.
- **TLS** is the newer, more secure version of SSL. (SSL 2.0/3.0 are outdated; today, TLS 1.2 and TLS 1.3 are used.)
**Main goals of SSL/TLS:**
- **Encryption:** Data is unreadable if intercepted.
- **Authentication:** Ensures you're talking to the real server (not a fake one).
- **Integrity:** Ensures the data isn’t changed or corrupted during transfer.
---
# **SSL Termination**
- **SSL Termination** means **decrypting** the SSL/TLS traffic **at a load balancer** (or proxy) **before** it reaches your backend servers.
**In simple words:**  
→ The **load balancer** handles the heavy work of decryption, so your servers can focus only on application logic.
**Why SSL Termination?**
- **Performance Boost:** Servers don’t waste CPU on decrypting HTTPS traffic.
- **Simplified Certificate Management:** Only the load balancer needs SSL certificates.
- **Easier Application Development:** Backend can use simple HTTP internally.
**Example:**  
You visit `https://mywebsite.com`  
1. Your browser talks securely to the Load Balancer (SSL/TLS).  
2. The Load Balancer decrypts the request.  
3. The Load Balancer sends normal HTTP traffic to the servers.
---
Good question!  
👉 **SSL/TLS encrypts the *entire* HTTPS request and response**, not just passwords or credit card info.
That means:
- URL paths (except domain name)
- Form data (like login info)
- API request bodies
- Cookies
- Query parameters (after the domain)
- Response data (like HTML, JSON)
**Example:**  
If you send a login request over HTTPS:
```
POST /login HTTP/1.1
Host: example.com
Content-Type: application/json
{ "username": "you", "password": "secret123" }
```
**→ This whole thing is encrypted**, so attackers can't see the username, password, headers, body — nothing!
The **only thing visible** without decryption is the **domain name** (example.com), because DNS needs it to route the traffic.
---
