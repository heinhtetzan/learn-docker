# 🌐 **1. Networking Basics**

### **What is a Network?**

A network is a group of computers/devices connected to share data. Each device has:

* **IP address:** Unique identifier on a network.
* **MAC address:** Physical hardware address.
* **Gateway / Router:** Device that forwards traffic to other networks.

### **Check Network Interfaces**

```bash
ifconfig          # old, shows interfaces and IP
ip addr show      # modern command
```

**Explanation:** Shows all network interfaces, IP addresses, MAC addresses, and status (up/down).

### **Bring interface up/down**

```bash
sudo ip link set eth0 up
sudo ip link set eth0 down
```

**Explanation:** Enables or disables a network interface.

---

# 🔌 **2. Ports**

### **What is a Port?**

* A **port** is a logical endpoint for network connections.
* Each service listens on a port, e.g.:

  * HTTP → 80
  * HTTPS → 443
  * SSH → 22
* Ports allow multiple services to run on one IP.

### **Check open/listening ports**

```bash
sudo ss -tuln
```

* `-t` TCP
* `-u` UDP
* `-l` Listening
* `-n` Numeric (no DNS)

**Example Output:**

```
LISTEN 0 128 0.0.0.0:22 0.0.0.0:*   # SSH server listening on port 22
```

### **Test remote port**

```bash
nc -zv example.com 22
telnet example.com 80
```

**Explanation:** Checks if a port is open/reachable on a remote host.

---

# 📶 **3. Ping / Connectivity**

### **Ping a host**

```bash
ping google.com
ping -c 4 8.8.8.8
```

**Explanation:** Sends ICMP packets to check if a host is reachable and measure latency.

### **Traceroute**

```bash
traceroute google.com
```

**Explanation:** Shows the path packets take to reach a host (helpful for diagnosing network issues).

---

# 🌐 **4. HTTP & CURL**

### **What is HTTP?**

* HTTP (HyperText Transfer Protocol) is the protocol for the web.
* HTTPS is the secure version (encrypted).

### **Check website with curl**

```bash
curl http://example.com          # GET request
curl -I http://example.com       # show headers only
curl -X POST -d "key=value" http://example.com/form
```

**Explanation:**

* `curl` sends requests to HTTP/HTTPS servers.
* You can test response, headers, download files, or send data.

### **Download a file**

```bash
curl -O http://example.com/file.zip
```

---

# 🔒 **5. SSH (Secure Shell)**

### **What is SSH?**

* SSH is a secure protocol to access remote servers over the network.
* Uses encryption to protect login credentials and data.

### **Connect to a remote server**

```bash
ssh user@192.168.1.10
ssh -p 2222 user@remotehost  # if SSH runs on custom port
```

### **Copy files over SSH**

```bash
scp file.txt user@remotehost:/path/to/destination
```

### **Generate SSH key (for passwordless login)**

```bash
ssh-keygen -t rsa -b 4096
ssh-copy-id user@remotehost
```

---

# 🔥 **1. Firewall**

### **What is a Firewall?**

* A firewall controls **incoming and outgoing network traffic** based on rules.
* Protects your system from unwanted access.

### **Linux Firewall Tools**

* **iptables** (classic)
* **ufw** (Uncomplicated Firewall, Ubuntu-friendly)
* **firewalld** (Fedora/RedHat-friendly)

### **Basic Commands (UFW)**

```bash
sudo ufw status               # Check firewall status
sudo ufw enable               # Enable firewall
sudo ufw disable              # Disable firewall
sudo ufw allow 22/tcp         # Allow SSH port
sudo ufw deny 80/tcp          # Block HTTP port
sudo ufw delete allow 22/tcp  # Remove rule
```

### **Explanation:**

Firewall rules determine **which ports/services are allowed or blocked**.

---

# 🛡 **2. Proxy**

### **What is a Proxy?**

* A proxy server acts as an **intermediary between your computer and the internet**.
* Benefits: privacy, caching, access control, bypass restrictions.

### **Types**

1. **Forward Proxy** – Client → Proxy → Internet
   Example: your computer uses a proxy to access websites.
2. **Reverse Proxy** – Internet → Proxy → Server
   Example: Nginx or Apache directs traffic to multiple backend servers.

### **Example (Set HTTP Proxy in Linux)**

```bash
export http_proxy=http://proxy.example.com:8080
export https_proxy=https://proxy.example.com:8080
```

### **Check proxy**

```bash
echo $http_proxy
curl -I http://example.com   # traffic goes through proxy
```

---

# 🔄 **3. Reverse Proxy**

### **What is a Reverse Proxy?**

* Receives requests from clients and **forwards them to backend servers**.
* Benefits: load balancing, SSL termination, caching, security.

### **Common Reverse Proxy Tools**

* **Nginx**
* **Apache**
* **HAProxy**

### **Example Nginx config**

```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://127.0.0.1:3000;  # forward to backend app
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

**Explanation:**

* Client requests → Nginx → backend app → response → client.
* Common for web apps with multiple services.

---

# 🌐 **4. VPN (Virtual Private Network)**

### **What is a VPN?**

* A VPN creates a **secure, encrypted tunnel** between your device and another network.
* Benefits: privacy, remote access to internal networks, bypass geo-restrictions.

### **Common VPN Types**

* **OpenVPN** (Open-source, widely used)
* **WireGuard** (Lightweight, high-performance)
* **IPSec / L2TP / PPTP** (Older protocols)

### **Example Commands (WireGuard)**

```bash
sudo wg-quick up wg0       # Start VPN
sudo wg-quick down wg0     # Stop VPN
sudo wg show               # Show VPN status
```

### **Check VPN IP**

```bash
curl ifconfig.me   # see public IP before/after VPN
```

---
