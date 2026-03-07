# Domain Name System (DNS)

## What is DNS?

The **Domain Name System (DNS)** is a core component of the Internet that translates **human-readable domain names** into **IP addresses** that computers use to communicate.

Example:

```
www.google.com → 142.250.190.78
```

Humans find it easier to remember names like `google.com`, while computers communicate using numerical IP addresses. DNS works as the **Internet's phonebook**, allowing users to access websites using domain names.

---

# DNS Services

DNS provides several important services:

## 1. Name Resolution
Translates domain names into IP addresses.

Example:
```
www.example.com → 93.184.216.34
```

---

## 2. Host Aliasing
Allows multiple domain names to refer to the same host.

Example:

```
example.com
www.example.com
ftp.example.com
```

---

## 3. Mail Server Aliasing
DNS allows email routing using **MX (Mail Exchange) records**.

Example:

```
example.com → mail.example.com
```

---

## 4. Load Distribution
DNS can return **multiple IP addresses** for a domain to distribute traffic across several servers.

Example:

```
server1.example.com → 192.168.1.1
server2.example.com → 192.168.1.2
server3.example.com → 192.168.1.3
```

This technique is often used by large services like CDNs and cloud platforms.

---

# Distributed Hierarchical Database

DNS is implemented as a **distributed hierarchical database**.

Instead of storing all domain information in one central server, the DNS system distributes the database across **many servers worldwide**.

## Advantages

- Scalability
- High availability
- Fault tolerance
- Faster query resolution
- Reduced network congestion

## DNS Hierarchy Structure

DNS is organized as an **inverted tree structure**.

```
                Root (.)
                  |
        -----------------------
        |          |         |
       .com       .org      .net
        |
      example
        |
       www
```

Each level delegates authority to the next.

---

# DNS Hierarchy Levels

## 1. Root Servers

The **root servers** are the highest level in the DNS hierarchy.

Characteristics:

- Represented by `.`
- Direct queries to the appropriate **Top-Level Domain (TLD)** server
- There are **13 logical root server clusters worldwide**

Example query:

```
Where can I find the .com servers?
```

---

## 2. Top-Level Domain (TLD) Servers

TLD servers manage domain extensions such as:

```
.com
.org
.net
.edu
.gov
.gr
.uk
```

They store information about **authoritative name servers** for domains.

Example query:

```
Where is example.com?
```

---

## 3. Authoritative Name Servers

Authoritative servers contain the **actual DNS records** for a domain.

They provide the final answer for DNS queries.

Example:

```
www.example.com → 93.184.216.34
```

Examples of stored records:

- A records
- AAAA records
- MX records
- CNAME records
- TXT records

---

## 4. Local DNS Servers (Recursive Resolvers)

Local DNS servers act as intermediaries between users and the DNS hierarchy.

They are usually operated by:

- Internet Service Providers (ISP)
- Companies
- Public DNS providers

Examples:

```
Google DNS → 8.8.8.8
Cloudflare DNS → 1.1.1.1
```

Responsibilities:

- Receive DNS queries from users
- Perform recursive queries
- Cache results for faster future responses

---

# Example of DNS Resolution

Suppose a user visits:

```
www.example.com
```

The DNS resolution process works as follows:

### Step 1 – Browser Cache
The browser first checks if it already knows the IP address.

---

### Step 2 – Local DNS Resolver
If not cached, the request goes to the **local DNS server**.

---

### Step 3 – Root Server Query

The resolver asks the root server:

```
Where are the .com servers?
```

---

### Step 4 – TLD Server Query

The resolver asks the `.com` TLD server:

```
Where is example.com?
```

---

### Step 5 – Authoritative Server Query

The authoritative server responds with:

```
www.example.com → 93.184.216.34
```

---

### Step 6 – Response Returned

The resolver sends the IP address back to the browser and caches the result.

---

## DNS Resolution Diagram

```
User
 |
 | 1. Query www.example.com
 v
Local DNS Resolver
 |
 | 2. Ask Root Server
 v
Root Server
 |
 | 3. Return .com TLD
 v
TLD Server (.com)
 |
 | 4. Return Authoritative Server
 v
Authoritative DNS Server
 |
 | 5. Return IP Address
 v
Local Resolver → User
```

---

# DNS Records

DNS records define how domain names behave and where services are located.

| Record Type | Purpose |
|-------------|--------|
| A | Maps domain to IPv4 address |
| AAAA | Maps domain to IPv6 address |
| CNAME | Alias for another domain |
| MX | Mail server for domain |
| NS | Authoritative name server |
| TXT | Text information (often used for security) |

---

## A Record (Address Record)

Maps a domain name to an **IPv4 address**.

Example:

```
example.com → 93.184.216.34
```

---

## AAAA Record

Maps a domain name to an **IPv6 address**.

Example:

```
example.com → 2606:2800:220:1:248:1893:25c8:1946
```

---

## CNAME Record (Canonical Name)

Creates an alias for another domain name.

Example:

```
www.example.com → example.com
```

---

## MX Record (Mail Exchange)

Specifies mail servers responsible for receiving emails.

Example:

```
example.com → mail.example.com
```

---

## NS Record (Name Server)

Specifies authoritative DNS servers for a domain.

Example:

```
example.com → ns1.example.com
```

---

## TXT Record

Stores text information often used for:

- Domain verification
- Email security
- SPF/DKIM records

Example:

```
v=spf1 include:_spf.google.com ~all
```

---
## DNS Message Sections
Every DNS message, whether a query or a response, is composed of up to five sections.


| Section | Content | Role |
| :--- | :--- | :--- |
| **Header** | Flags & IDs | Controls the transaction (e.g., is this a query or response?). |
| **Question** | Query parameters | Contains the target domain name and the requested record type. |
| **Answer** | Actual Records | Contains the RRs that directly answer the query. |
| **Authority** | Server Info | Points to authoritative name servers for the domain. |
| **Additional** | Helpful Data | Provides extra info, like IP addresses for the servers in the Authority section. |

---
## Resource Record (RR) Structure
Inside a DNS message, every record in the Answer, Authority, and Additional sections follows this binary format:


| Field | Size | Purpose |
| :--- | :--- | :--- |
| **NAME** | Variable | The domain name the record pertains to (often compressed). |
| **TYPE** | 2 Bytes | The record type ID (e.g., Type 1 for 'A', Type 15 for 'MX'). |
| **CLASS** | 2 Bytes | Almost always `1` (IN - Internet). |
| **TTL** | 4 Bytes | **Time-To-Live**: How many seconds the record can be cached. |
| **RDLENGTH** | 2 Bytes | The length of the RDATA field in bytes. |
| **RDATA** | Variable | The record's specific data (e.g., the IP address or alias). |

---
# DNS Attacks

Because DNS is critical infrastructure, it is frequently targeted by attackers.

---

## 1. DNS Spoofing (Cache Poisoning)

Attackers insert fake DNS records into a resolver cache.

Example:

```
bank.com → attacker IP
```

Users may be redirected to malicious websites.

---

## 2. DNS Amplification Attack

A type of **Distributed Denial of Service (DDoS)** attack.

Attack process:

1. Attacker sends small DNS queries
2. Spoofs the victim's IP
3. DNS servers send large responses to the victim

Result: network overload.

---

## 3. DNS Tunneling

Attackers use DNS queries to secretly transfer data.

Common uses:

- Data exfiltration
- Command and control communication

---

## 4. NXDOMAIN Attack

Attackers send massive queries for **non-existent domains**, overwhelming DNS servers.

---

# DNS Security

Several technologies improve DNS security.

---

## 1. DNSSEC (DNS Security Extensions)

DNSSEC adds **cryptographic signatures** to DNS records.

Provides:

- Authentication
- Data integrity
- Protection against spoofing

---

## 2. DNS over HTTPS (DoH)

Encrypts DNS queries using HTTPS.

Benefits:

- Improved privacy
- Protection against eavesdropping

---

## 3. DNS over TLS (DoT)

Encrypts DNS communication using TLS.

Similar goal as DoH but uses a different transport protocol.

---

## 4. Rate Limiting

DNS servers limit response rates to reduce **amplification attacks**.

---

# Summary

DNS is one of the most important services on the Internet.

Key concepts:

- DNS translates **domain names into IP addresses**
- Uses a **distributed hierarchical architecture**
- Consists of **Root, TLD, Authoritative, and Local DNS servers**
- Uses **DNS records** to define domain information
- Vulnerable to attacks such as **DNS spoofing and amplification**
- Security improvements include **DNSSEC, DoH, and DoT**

---

# References

- RFC 1034 – Domain Names: Concepts and Facilities  
- RFC 1035 – Domain Names: Implementation and Specification  
- ICANN Documentation  
- Cloudflare DNS Learning Center
 
