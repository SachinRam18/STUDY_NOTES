# Q1. What is JWT? Explain how JWT works.

→ **JWT (JSON Web Token)** is a compact format for carrying claims between parties. It is commonly used as a bearer token after a user signs in, so the client does not send the password with every request.

## JWT Structure

A signed JWT contains **3 parts**:

```text
Header.Payload.Signature
```

### 1. Header

→ Contains token information such as the signing algorithm and type.

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

### 2. Payload

→ Contains **claims** about the user/token.

```json
{
  "sub": "12345",
  "role": "USER",
  "exp": 1790000000
}
```

→ Common claims: `sub`, `iat`, `exp`, `iss`, `aud`.

→ Header and payload are **Base64URL encoded, not encrypted**, so sensitive information/passwords should not be stored in them.

### 3. Signature

→ Used to verify that the token was not modified and was signed using the expected key.

For HS256:

```text
Signature = HMAC-SHA256(secretKey, encodedHeader + "." + encodedPayload)
```

→ The secret key is securely stored on the server and is **not sent to the client**.

## JWT Flow

```text
Client → Login credentials → Server
Server → Verify credentials
Server → Create Header + Payload
Server → Sign JWT using secret key
Server → Send JWT → Client

Client → Authorization: Bearer <JWT> → Server
Server → Verify signature + claims
Server → Check authorization
Server → Allow / Reject request
```

→ JWT provides authentication/integrity, while **authorization** determines whether the authenticated user can perform a particular action.

# Q2. How does React communicate with FastAPI and SQL?

→ React acts as the **client/frontend**, FastAPI as the **backend/API**, and SQL database stores the data.

## React

```jsx
const response = await fetch('http://localhost:8000/users/1')
const user = await response.json()
```

→ React sends an HTTP `GET` request to FastAPI.

## FastAPI

```python
@app.get("/users/{user_id}")
def get_user(user_id: int):
    user = connection.execute(
        "SELECT name, contact, email_id FROM users WHERE id = ?",
        (user_id,)
    ).fetchone()

    return dict(user)
```

→ FastAPI receives the `user_id`.
→ It executes a **parameterized SQL query** to retrieve the user.
→ The database returns the matching row.
→ FastAPI converts it into JSON and sends it back to React.

## SQL

```sql
SELECT name, contact, email_id
FROM users
WHERE id = ?;
```

→ `?` is a parameter placeholder, which helps prevent SQL injection.

## Complete Flow

```text
React
  ↓ HTTP GET /users/1
FastAPI
  ↓ SQL Query
Database
  ↓ User Row
FastAPI
  ↓ JSON Response
React
```

**Interview explanation:**  
→ React sends the request → FastAPI handles the API → FastAPI queries the SQL database → database returns the data → FastAPI sends JSON → React displays the data.

# DNS (Domain Name System)

## What is DNS?

→ **DNS (Domain Name System)** translates human-readable domain names such as `google.com` into **IP addresses** such as `142.250.x.x`.

→ Computers communicate using IP addresses, while users remember domain names.

→ DNS works like the **phonebook of the Internet**.

```text
google.com
    ↓ DNS
IP Address
    ↓
142.250.x.x
```

---

## DNS Resolution

→ **DNS resolution** is the process of finding the IP address associated with a domain name.

Example:

```text
User enters:
www.example.com

        ↓

DNS Resolution

        ↓

IP Address:
93.184.216.34

        ↓

Browser connects to the server
```

---

## Main Components of DNS

The important components involved in DNS resolution are:

1. **DNS Resolver**
2. **Root DNS Server**
3. **TLD Name Server**
4. **Authoritative DNS Server**

```text
Client
   ↓
DNS Resolver
   ↓
Root Server
   ↓
TLD Server
   ↓
Authoritative Server
```

---

### 1. DNS Resolver

→ The **DNS Resolver** is the server that receives the DNS query from the client.

→ It is usually provided by the ISP, organization, or a public DNS provider.

Examples:

```text
Google DNS      → 8.8.8.8
Cloudflare DNS  → 1.1.1.1
```

→ The resolver performs the lookup on behalf of the client.

→ It also maintains a **cache** of previously resolved domain names.

---

### 2. Root DNS Server

→ The **Root DNS servers** are at the top of the DNS hierarchy.

→ They do not normally provide the IP address of `example.com`.

→ Instead, they tell the resolver which **TLD server** handles the domain.

For example:

```text
example.com
     ↓
Root Server
     ↓
".com" TLD Server
```

→ There are **13 logical root server identities**, operated using many physical servers distributed around the world.

---

### 3. TLD Name Server

→ **TLD = Top-Level Domain**.

Examples:

```text
.com
.org
.net
.in
.uk
```

→ The TLD server knows which **authoritative DNS server** is responsible for a particular domain.

Example:

```text
example.com
     ↓
.com TLD Server
     ↓
Authoritative Server for example.com
```

→ The TLD server generally does not provide the final IP address.

---

### 4. Authoritative DNS Server

→ The **authoritative DNS server** contains the actual DNS records for a domain.

Example:

```text
example.com
      ↓
Authoritative DNS Server
      ↓
A Record → 93.184.216.34
```

→ It provides the final answer to the resolver.

→ Common DNS records include:

```text
A       → IPv4 address
AAAA    → IPv6 address
CNAME   → Alias of another domain
MX      → Mail server
NS      → Name server
TXT     → Text information
```

---

## DNS Hierarchy

DNS is hierarchical.

```text
                    Root
                     .
                     ↓
              TLD Servers
          .com   .org   .in
             ↓
       Authoritative Servers
             ↓
        example.com
             ↓
          DNS Records
```

For:

```text
www.example.com
```

The hierarchy is:

```text
. → com → example → www
```

→ `.` is the root.
→ `com` is the TLD.
→ `example.com` is the domain.
→ `www` is the hostname/subdomain.

---

## Types of DNS Servers

### 1. Recursive Resolver

→ Receives the query from the client and finds the final answer.

```text
Client → Resolver
```

→ It may contact root, TLD, and authoritative servers.

---

### 2. Root Name Server

→ Directs queries to the appropriate TLD server.

```text
Root → .com / .org / .in
```

---

### 3. TLD Name Server

→ Directs queries to the authoritative server for the requested domain.

```text
.com TLD → example.com's authoritative server
```

---

### 4. Authoritative Name Server

→ Stores and returns the actual DNS records for the domain.

```text
example.com → IP address
```

---

## Standard DNS Resolution Flow

Suppose the user enters:

```text
www.example.com
```

### Step 1 — Browser Cache

→ Browser first checks whether it already knows the IP address.

```text
Browser Cache
      ↓
IP found?
```

→ If found and still valid, DNS lookup may not be required.

---

### Step 2 — Operating System Cache

→ If the browser does not have the answer, the operating system may check its DNS cache.

```text
Browser
   ↓
OS DNS Cache
```

---

### Step 3 — DNS Resolver

→ If the IP is not cached locally, the request goes to the configured **recursive DNS resolver**.

```text
Client
   ↓
DNS Resolver
```

→ The resolver also checks its own cache.

---

### Step 4 — Root Server

→ If the resolver does not have the answer, it queries a root DNS server.

```text
Resolver
    ↓
Root Server
```

→ Root server responds:

```text
"I don't know the IP,
but ask the .com TLD server."
```

---

### Step 5 — TLD Server

→ Resolver queries the `.com` TLD server.

```text
Resolver
    ↓
.com TLD Server
```

→ The TLD server responds with the authoritative name server for `example.com`.

```text
.com
 ↓
Authoritative Server
```

---

### Step 6 — Authoritative Server

→ Resolver queries the authoritative server.

```text
Resolver
    ↓
Authoritative DNS Server
```

→ The authoritative server returns the actual DNS record.

```text
www.example.com
        ↓
93.184.216.34
```

---

### Step 7 — Resolver Returns IP

→ Resolver sends the IP address back to the client.

```text
Authoritative Server
        ↓
DNS Resolver
        ↓
Client
```

---

### Step 8 — Browser Connects

→ Now the browser knows the server's IP address.

```text
www.example.com
        ↓
93.184.216.34
        ↓
TCP / TLS
        ↓
HTTP/HTTPS Request
```

---

## Complete DNS Resolution Flow

```text
User enters www.example.com
            ↓
      Browser Cache
            ↓
       OS DNS Cache
            ↓
      DNS Resolver
            ↓
        Root Server
            ↓
        .com TLD
            ↓
 Authoritative Name Server
            ↓
       DNS Record
            ↓
     IP Address returned
            ↓
       DNS Resolver
            ↓
          Client
            ↓
 Browser connects to server
```

---

## Recursive vs Iterative Query

### Recursive Query

→ Client asks the resolver:

```text
"What is the IP of example.com?"
```

→ Resolver is expected to return the final answer.

```text
Client → Resolver
             ↓
       Finds the answer
             ↓
Client ← IP Address
```

---

### Iterative Query

→ The resolver asks DNS servers step-by-step.

```text
Resolver → Root
Root → TLD

Resolver → TLD
TLD → Authoritative Server

Resolver → Authoritative
Authoritative → IP
```

→ Each server gives the best information it knows, such as a referral to the next server.

---

## DNS Caching

→ DNS responses are cached to avoid performing the complete lookup every time.

```text
Client
  ↓
Resolver Cache
  ↓
If found → Return IP
If not found → Perform DNS lookup
```

→ DNS records have a **TTL (Time To Live)**.

Example:

```text
example.com → 93.184.216.34
TTL = 3600 seconds
```

→ The resolver can cache the result for the permitted TTL.

---

## DNS Record Types

| Record  | Purpose                                 |
| ------- | --------------------------------------- |
| `A`     | Maps domain → IPv4 address              |
| `AAAA`  | Maps domain → IPv6 address              |
| `CNAME` | Maps one hostname to another hostname   |
| `MX`    | Specifies mail servers                  |
| `NS`    | Specifies authoritative name servers    |
| `TXT`   | Stores text information                 |
| `PTR`   | Used for reverse DNS                    |
| `SOA`   | Contains information about the DNS zone |

Example:

```text
example.com
     ↓
A → 93.184.216.34
```

---

## Forward vs Reverse DNS

### Forward DNS

→ Converts:

```text
Domain → IP
```

Example:

```text
google.com → IP address
```

→ Usually uses `A` or `AAAA` records.

---

### Reverse DNS

→ Converts:

```text
IP → Domain
```

→ Uses a `PTR` record.

```text
IP Address
    ↓
PTR
    ↓
hostname
```

---

## DNS and HTTP Are Different

→ DNS only helps find the server's IP address.

```text
DNS:
example.com → IP
```

→ After DNS resolution, the browser establishes a network connection and sends the HTTP/HTTPS request.

```text
DNS
 ↓
IP Address
 ↓
TCP / QUIC
 ↓
TLS (for HTTPS)
 ↓
HTTP Request
 ↓
Web Server
```

---

## Interview Explanation

**Question: Explain what happens when you enter a URL in the browser.**

→ When I enter `www.example.com`, the browser first checks its cache, followed by the operating system's DNS cache. If the IP is not available, the request goes to the recursive DNS resolver. The resolver checks its cache and, if necessary, queries the DNS hierarchy starting from the **root server**, then the **TLD server**, and finally the **authoritative DNS server**. The authoritative server returns the DNS record containing the IP address. The resolver returns the IP to the client, and the browser can then establish a connection with that server and send the HTTP/HTTPS request.

---

## One-Line DNS Flow

```text
Domain
  ↓
Browser/OS Cache
  ↓
Recursive Resolver
  ↓
Root Server
  ↓
TLD Server
  ↓
Authoritative Server
  ↓
IP Address
  ↓
Browser connects to server
```
# 🌐 Computer Networks — What Happens When You Search "Iron Man"

## 1. User Enters Query

```text
User
 ↓
Browser
 ↓
Searches: "Iron Man"
```

The browser creates a search request:

```text
https://www.google.com/search?q=Iron+Man
```

---

## 2. DNS Resolution

The browser needs Google's **IP address**.

```text
Browser
   ↓
DNS Resolver
   ↓
Root DNS Server
   ↓
.com TLD Server
   ↓
Google Authoritative DNS
   ↓
Google IP Address
```

**DNS = Domain Name → IP Address**

Example:

```text
google.com → IP Address
```

### Important

DNS is used to find the server's IP address.  
After getting the IP, the browser can communicate with the server.

---

## 3. TCP Connection

Before sending HTTP data, a TCP connection is established.

### TCP 3-Way Handshake

```text
Client                    Server
  |                         |
  | ------ SYN ----------> |
  | <----- SYN + ACK ----- |
  | ------ ACK ----------> |
  |                         |
  |   Connection Ready      |
```

### Meaning

- **SYN** → Client requests a connection
- **SYN + ACK** → Server accepts and responds
- **ACK** → Client confirms

After this, the TCP connection is established.

---

## 4. TLS Handshake

Since Google uses **HTTPS**, TLS is established before application data is exchanged.

```text
Client
  ↓
TLS Handshake
  ↓
Server Certificate
  ↓
Key Exchange
  ↓
Secure Encrypted Connection
```

TLS provides:

- **Encryption**
- **Server authentication**
- **Data integrity**

```text
HTTP + TLS = HTTPS
```

---

## 5. HTTP Request

The browser sends an HTTP request containing the search query.

Example:

```http
GET /search?q=Iron+Man HTTP/1.1
Host: www.google.com
```

Flow:

```text
Browser
   ↓
HTTP Request
   ↓
TCP
   ↓
Internet
   ↓
Google Server
```

---

## 6. Server Processing

Google receives the request.

```text
Google Infrastructure
        ↓
   Query Processing
        ↓
     Search Index
        ↓
  Relevant Results
        ↓
      Ranking
```

The query:

```text
"Iron Man"
```

is processed to find relevant search results.

---

## 7. HTTP Response

Google sends the response back to the browser.

```text
Google Server
     ↓
HTTP Response
     ↓
Internet
     ↓
Browser
```

Example:

```http
HTTP/1.1 200 OK
Content-Type: text/html
```

The response contains the data required to display the search page.

---

## 8. Browser Renders the Page

The browser processes the received response.

```text
HTTP Response
      ↓
Parse HTML
      ↓
Load CSS + JavaScript
      ↓
Render Page
      ↓
Search Results
```

Finally, the user sees the search results.

---

## 🔥 Complete CN Flow

```text
User searches "Iron Man"
          ↓
       Browser
          ↓
    DNS Resolution
          ↓
      IP Address
          ↓
   TCP 3-Way Handshake
          ↓
     TLS Handshake
          ↓
      HTTP GET
          ↓
   Internet / Network
          ↓
 Google Infrastructure
          ↓
 Query Processing
          ↓
 Search Index + Ranking
          ↓
    HTTP Response
          ↓
       Browser
          ↓
    Page Rendering
          ↓
    Search Results
```

---

## 🎯 Interview Answer

When I search **"Iron Man"**, the browser first performs **DNS resolution** to obtain Google's IP address. Then it establishes a **TCP connection using the 3-way handshake**. Since Google uses HTTPS, a **TLS handshake** is performed to create a secure connection. The browser then sends an **HTTP GET request** containing the search query. Google's infrastructure processes the query, retrieves relevant results from its search index, and ranks them. The server sends an **HTTP response** back, and the browser renders the search results.

---

## 🧠 Remember

```text
DNS
 ↓
IP Address
 ↓
TCP
 ↓
TLS
 ↓
HTTP
 ↓
Server Processing
 ↓
HTTP Response
 ↓
Browser
```

### One-Line Flow

```text
DNS → TCP → TLS → HTTP → Server → HTTP Response → Browser
```

# SQL vs NoSQL — Interview POV

| SQL | NoSQL |
|---|---|
| Relational tables and a defined schema are common | Includes document, key-value, graph, and wide-column models |
| Supports joins and expressive relational queries | Data access patterns depend on the database model |
| Strong transaction support is common | Transaction and consistency guarantees vary by database |
| Often suits relational data and complex queries | Often suits specific access patterns or distributed workloads |

# PUT vs PATCH

| **PUT**                             | **PATCH**                           |
| ----------------------------------- | ----------------------------------- |
| Replaces the **entire resource**    | Updates **part of a resource**      |
| Usually sends **all fields**        | Sends **only fields to be changed** |
| Idempotent: repeating the same request has the same intended effect | May or may not be idempotent; it depends on the patch operation |
| Used for **complete updates**       | Used for **partial updates**        |
| Example: Update entire user profile | Example: Update only user's email   |
| **PUT /users/1**                    | **PATCH /users/1**                  |

# React Hooks

Hooks let function components use React features such as state and effects.
Call Hooks only at the top level of a function component or a custom Hook.

| Hook | Common use |
|---|---|
| `useState` | Store component state |
| `useEffect` | Synchronize with external systems after rendering |
| `useContext` | Read a value provided by a context |
| `useRef` | Hold a mutable value or DOM reference without causing a re-render |

## `useState`

`useState` returns the current state and a setter. Calling the setter schedules
a re-render with the new state.

```jsx
const [count, setCount] = useState(0);

function increment() {
    setCount(currentCount => currentCount + 1);
}
```

Use the functional setter when the next value depends on the previous value.

**Interview point:** The setter schedules an update; it does not change the
state value in the already-running render.

## `useEffect`

`useEffect` synchronizes a component with an external system after a render.
Common examples include subscriptions, timers, and browser APIs. For user
actions such as submitting a form, put the action in the event handler rather
than using an Effect as an indirect trigger.

```jsx
useEffect(() => {
    // Set up synchronization here.

    return () => {
        // Clean up before the Effect runs again or the component unmounts.
    };
}, [dependencies]);
```

### Dependency patterns

| Dependency list | When the Effect runs |
|---|---|
| Omitted | After every render |
| `[]` | After the component mounts |
| `[count]` | After mount and when `count` changes |

Include every reactive value used by the Effect in its dependency list.

### Example: timer with cleanup

```jsx
useEffect(() => {
    const timerId = setInterval(() => {
        console.log("Running");
    }, 1000);

    return () => clearInterval(timerId);
}, []);
```

**Why cleanup matters:** It prevents stale subscriptions, timers, or event
listeners from continuing after they are no longer needed.

**Interview question:** What is the difference between `useState` and `useEffect`?
**Short answer:** `useState` stores component data and schedules re-renders;
`useEffect` synchronizes with external systems after rendering.
