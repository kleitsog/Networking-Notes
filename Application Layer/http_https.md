# HTTP, HTTPS, and Cookies --- A Practical Technical Overview

## 1. What is HTTP?

**HTTP (HyperText Transfer Protocol)** is an application-layer protocol
used for communication between clients and servers on the web.

It follows a **request--response model**:

1.  A **client** (browser, mobile app, API client) sends a request.
2.  A **server** processes the request.
3.  The server returns a **response**.

HTTP is:

-   Stateless
-   Text-based (HTTP/1.1)
-   Designed for distributed systems
-   Extensible via headers

Official specification family: **RFC 9110--9114** (HTTP Semantics and
HTTP/1.1/2).

------------------------------------------------------------------------

# 2. HTTP Communication Model

    Client (Browser)
          │
          │ HTTP Request
          ▼
    Server (Web Server / API)
          │
          │ HTTP Response
          ▼
    Client

A single interaction is called an **HTTP transaction**.

------------------------------------------------------------------------

# 3. HTTP Request Structure

An HTTP request contains:

1.  **Request Line**
2.  **Headers**
3.  **Empty line**
4.  **Optional Body**

## Request Format

    METHOD SP REQUEST_TARGET SP HTTP_VERSION
    Header-Name: value
    Header-Name: value

    [optional body]

## Example HTTP Request

    GET /api/users?id=10 HTTP/1.1
    Host: example.com
    User-Agent: Mozilla/5.0
    Accept: application/json
    Connection: keep-alive

### Explanation

 | Part | Meaning |
 |------|--------|
 | GET | HTTP method |
 | /api/users?id=10 | resource path |
 | HTTP/1.1 | protocol version |
 | Host | domain name |
 | User-Agent | client identifier |
 | Accept | expected response format |

------------------------------------------------------------------------

# 4. HTTP Methods

Common HTTP methods:

 | Method | Purpose |
 |------|------|
 | GET | Retrieve data |
 | POST | Send data to server |
 | PUT | Replace resource |
 | PATCH | Partial update |
 | DELETE | Remove resource |
 | HEAD | Same as GET without body |
 | OPTIONS | Discover supported methods |

Example:

    POST /api/login HTTP/1.1
    Host: example.com
    Content-Type: application/json
    Content-Length: 44

    {
      "username": "alice",
      "password": "secret"
    }

------------------------------------------------------------------------

# 5. HTTP Response Structure

A response contains:

1.  **Status Line**
2.  **Headers**
3.  **Empty Line**
4.  **Body**

## Response Format

    HTTP_VERSION STATUS_CODE REASON_PHRASE
    Header-Name: value
    Header-Name: value

    [response body]

## Example HTTP Response

    HTTP/1.1 200 OK
    Content-Type: application/json
    Content-Length: 58
    Set-Cookie: session_id=abc123; HttpOnly; Secure

    {
      "id": 10,
      "name": "Alice"
    }

### Explanation

  | Part | Meaning |
 |------|--------|
 | HTTP/1.1 | protocol version |
 | 200 | status code |
 | OK | reason phrase |
 | Content-Type | data format |
 | Set-Cookie | instructs browser to store cookie |

------------------------------------------------------------------------

# 6. HTTP Headers

Headers add metadata to requests and responses.

Examples:

 | Header | Description |
 |------|------|
 | Host | Domain name |
 | User-Agent | Client software |
 | Accept | Expected response format |
 | Content-Type | Format of body |
 | Content-Length | Body size |
 | Authorization | Authentication data |
 | Cookie | Sends stored cookies |
 | Set-Cookie | Server instructs browser to store cookie |
 
------------------------------------------------------------------------

# 7. HTTP Status Codes

Status codes indicate the result of the request.

## Status Code Categories

 | Range | Meaning |
 |------|------|
 | 1xx | Informational |
 | 2xx | Success |
 | 3xx | Redirection |
 | 4xx | Client Error |
 | 5xx | Server Error |
 
## Status Code Array

``` json
[
  {"code":100,"name":"Continue","type":"Informational"},
  {"code":101,"name":"Switching Protocols","type":"Informational"},

  {"code":200,"name":"OK","type":"Success"},
  {"code":201,"name":"Created","type":"Success"},
  {"code":202,"name":"Accepted","type":"Success"},
  {"code":204,"name":"No Content","type":"Success"},

  {"code":301,"name":"Moved Permanently","type":"Redirection"},
  {"code":302,"name":"Found","type":"Redirection"},
  {"code":304,"name":"Not Modified","type":"Redirection"},

  {"code":400,"name":"Bad Request","type":"Client Error"},
  {"code":401,"name":"Unauthorized","type":"Client Error"},
  {"code":403,"name":"Forbidden","type":"Client Error"},
  {"code":404,"name":"Not Found","type":"Client Error"},
  {"code":405,"name":"Method Not Allowed","type":"Client Error"},
  {"code":409,"name":"Conflict","type":"Client Error"},
  {"code":422,"name":"Unprocessable Entity","type":"Client Error"},

  {"code":500,"name":"Internal Server Error","type":"Server Error"},
  {"code":501,"name":"Not Implemented","type":"Server Error"},
  {"code":502,"name":"Bad Gateway","type":"Server Error"},
  {"code":503,"name":"Service Unavailable","type":"Server Error"},
  {"code":504,"name":"Gateway Timeout","type":"Server Error"}
]
```

------------------------------------------------------------------------

# 8. What is HTTPS?

**HTTPS (HyperText Transfer Protocol Secure)** is HTTP running over
**TLS (Transport Layer Security)**.

    HTTPS = HTTP + TLS

HTTPS provides:

-   Encryption
-   Authentication
-   Data integrity

Without HTTPS, traffic can be intercepted.

------------------------------------------------------------------------

# 9. TLS Handshake (Simplified)

When a browser connects to an HTTPS server:

1.  Client sends **ClientHello**
2.  Server responds with **ServerHello** + certificate
3.  Client verifies certificate
4.  Key exchange occurs
5.  Secure session established

After the handshake:

    Encrypted HTTP communication begins

------------------------------------------------------------------------

# 10. What are Cookies?

**Cookies** are small pieces of data stored by the browser and
associated with a domain.

They allow servers to maintain **state** in the otherwise stateless HTTP
protocol.

Typical uses:

-   Session management
-   Authentication
-   Tracking
-   Personalization

------------------------------------------------------------------------

# 11. Cookie Flow

## Step 1 --- Server Sends Cookie

    HTTP/1.1 200 OK
    Set-Cookie: session_id=abc123; Path=/; HttpOnly; Secure

## Step 2 --- Browser Stores Cookie

Browser associates cookie with domain.

## Step 3 --- Browser Sends Cookie Back

    GET /dashboard HTTP/1.1
    Host: example.com
    Cookie: session_id=abc123

------------------------------------------------------------------------

# 12. Cookie Attributes

 | Attribute | Purpose |
 |----------|---------|
 | Expires | expiration time |
 | Max-Age | lifetime in seconds |
 | Domain | allowed domain |
 | Path | path restriction |
 | Secure | only sent via HTTPS |
 | HttpOnly | inaccessible to JavaScript |
 | SameSite | controls cross-site sending |
 
Example:

    Set-Cookie: session_id=abc123;
               Path=/;
               Secure;
               HttpOnly;
               SameSite=Strict;
               Max-Age=3600

------------------------------------------------------------------------

# 13. Types of Cookies

## Session Cookies

-   Stored in memory
-   Deleted when browser closes

## Persistent Cookies

-   Stored on disk
-   Have expiration time

## Third‑Party Cookies

-   Set by different domain
-   Often used for advertising/tracking

------------------------------------------------------------------------

# 14. Security Considerations

## Cookie Theft

If cookies are stolen, attackers may hijack sessions.

Mitigation:

-   `Secure`
-   `HttpOnly`
-   `SameSite`

## Man-in-the-Middle Attacks

Without HTTPS:

-   Attackers can read/modify traffic

Mitigation:

-   Always use HTTPS

## Session Fixation

Attackers set known session IDs.

Mitigation:

-   Regenerate session IDs after login

------------------------------------------------------------------------

# 15. Summary

HTTP provides:

-   Stateless request-response communication

HTTPS adds:

-   Encryption
-   Authentication
-   Integrity

Cookies provide:

-   State management on top of HTTP

Together they form the core foundation of modern web communication.
