# HTTP vs HTTPS — Quick Interview Notes

## 1. Definitions

- **HTTP (HyperText Transfer Protocol)**  
  The foundational protocol for web communication. It enables data transfer (like HTML, images, videos) between browsers and web servers over the internet :contentReference[oaicite:0]{index=0}.

- **HTTPS (HyperText Transfer Protocol Secure)**  
  Essentially HTTP layered over **SSL/TLS**, enabling secure, encrypted communication between clients and servers :contentReference[oaicite:1]{index=1}.

---

## 2. Key Differences

| Feature            | HTTP                         | HTTPS (HTTP + SSL/TLS)                     |
|--------------------|------------------------------|--------------------------------------------|
| **Port**           | 80                           | 443 :contentReference[oaicite:2]{index=2}         |
| **Encryption**     | None — data in plaintext     | Encrypted — secure against eavesdropping :contentReference[oaicite:3]{index=3} |
| **Authentication** | No server validation         | Uses CA-signed certificates to verify identity :contentReference[oaicite:4]{index=4} |
| **Data Integrity** | No protection                | Protects against tampering & MITM attacks :contentReference[oaicite:5]{index=5} |
| **Performance**    | Faster (no crypto overhead)  | Slight overhead, but modern optimizations reduce impact :contentReference[oaicite:6]{index=6} |
| **SEO Impact**     | Unfavored by search engines  | Preferred—boosts rankings and trust :contentReference[oaicite:7]{index=7} |
| **Browser Signals**| No padlock, may show “Not Secure” :contentReference[oaicite:8]{index=8}  | Padlock icon, secure connection indication :contentReference[oaicite:9]{index=9} |

---

## 3. Working Mechanisms

### HTTP  
- Operates at the **application layer**.
- Stateless: each request is independent; no built-in session memory :contentReference[oaicite:10]{index=10}.

### HTTPS  
- Wraps HTTP within an encrypted SSL/TLS tunnel.
- Involves an SSL/TLS handshake: exchange of keys, establishment of secure session :contentReference[oaicite:11]{index=11}.

---

## 4. Why HTTPS Is Crucial

- **Privacy Protection**: Blocks passive eavesdropping (e.g., on public Wi-Fi) by encrypting data :contentReference[oaicite:12]{index=12}.
- **Security & Trust**: Helps verify server identity and prevents content tampering, impostor sites, or censorship :contentReference[oaicite:13]{index=13}.
- **Adoption Trends**: Increasingly the norm across all site types—not just e-commerce—thanks to initiatives like Let’s Encrypt :contentReference[oaicite:14]{index=14}.
- **Browser Enforcement**: Modern browsers flag HTTP sites as “Not Secure,” influencing user behavior and web standards :contentReference[oaicite:15]{index=15}.

---

## 5. Interview-Friendly Summary

- **HTTP = Plain**, **HTTPS = Secure (HTTP + TLS/SSL)**
- Always prefer **HTTPS**—for encryption, authentication, and integrity.
- SEO and user trust favor HTTPS-enabled sites.
- Knowledge of ports, SSL/TLS handshake, and browser security cues can give you an edge.

---

Let me know if you'd like to go deeper into the SSL/TLS handshake process, secure cipher suites, or how to set up HTTPS on servers—happy to help with that too!
::contentReference[oaicite:16]{index=16}


# HTTP Headers & Status Codes — Quick Reference Notes

---

## 1. HTTP Headers

**What Are They & How Do They Work?**  
HTTP headers are **key-value pairs** sent in both requests and responses. They follow the initial request/response line, separated by `CR+LF` (carriage return + line feed), and the header section concludes with a blank line :contentReference[oaicite:0]{index=0}.  

In HTTP/2 and HTTP/3, headers are **compressed using HPACK (HTTP/2)** or **QPACK (HTTP/3)** and transmitted as binary frames for efficiency :contentReference[oaicite:1]{index=1}.

**Why They Matter**:  
- Guide data encoding and content types (e.g., `Content-Type`, `Accept-Encoding`)  
- Handle caching (`Cache-Control`, `ETag`, `If-None-Match`)  
- Manage sessions and security (e.g., `Cookies`, `Authorization`)  
- Enable content negotiation and routing (`Accept-Language`, `Referer`, `Location`)  
- Enforce cross-origin policies (`Origin`, `Access-Control-*`)  
:contentReference[oaicite:2]{index=2}

:contentReference[oaicite:4]{index=4}

---

## 2. HTTP Status Codes

**Overview**:  
Status codes are **three-digit numbers**, categorized by their first digit:
- **1xx – Informational** (request accepted, process continues)  
- **2xx – Success** (request successful)  
- **3xx – Redirection** (further action required)  
- **4xx – Client Errors** (problem with request)  
- **5xx – Server Errors** (server cannot fulfill valid request)  
:contentReference[oaicite:5]{index=5}

**Key Status Codes to Know**:

```markdown
### 1xx – Informational  
100 Continue – continue sending request body  
101 Switching Protocols – agreed to change protocol (e.g., HTTP → WebSocket)  
103 Early Hints – suggest resources to preload while processing  
:contentReference[oaicite:6]{index=6}

### 2xx – Success  
200 OK – standard success response  
201 Created – resource successfully created (e.g., POST)  
202 Accepted – request accepted but not completed yet  
204 No Content – success, no content to return  
206 Partial Content – serving part of resource (e.g., byte ranges)  
:contentReference[oaicite:7]{index=7}

### 3xx – Redirection  
301 Moved Permanently – update bookmarks to new URL  
302 Found – temporary redirect  
303 See Other – directs to a new resource using GET  
304 Not Modified – use cached version of resource  
:contentReference[oaicite:8]{index=8}

### 4xx – Client Errors  
400 Bad Request – malformed or invalid request  
401 Unauthorized – authentication needed or failed  
403 Forbidden – authenticated but no permission to access resource  
404 Not Found – resource not found on server  
429 Too Many Requests – rate limiting in effect  
:contentReference[oaicite:9]{index=9}  
:contentReference[oaicite:10]{index=10}

### 5xx – Server Errors  
500 Internal Server Error – generic unexpected server error  
502 Bad Gateway – invalid response from upstream server  
503 Service Unavailable – overloaded or under maintenance server  
504 Gateway Timeout – upstream server did not respond in time  
:contentReference[oaicite:11]{index=11}
```
