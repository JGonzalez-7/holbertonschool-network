# What Happens When You Type `https://www.google.com` in Your Browser and Press Enter?

**Repository article URL:** [View this article on GitHub](https://github.com/JGonzalez-7/holbertonschool-network/blob/main/what_happens_when_your_type_google_com_in_your_browser_and_press_enter/0-blog_post.md)

**Medium or LinkedIn URL:** Add the public URL here after publishing this article.

Typing a URL and pressing Enter looks like one simple action. Behind it, however, the browser and several network services must locate a server, establish a reliable connection, protect the exchanged data, process the request, and render the response. The complete operation usually takes only a fraction of a second.

The following diagram summarizes the request:

![Flow of an HTTPS request from a browser to a web application](./request_flow.svg)

## 1. The Browser Parses the URL

The browser first separates the URL into its important components:

```text
https://www.google.com/
```

- `https` is the protocol scheme. It tells the browser to use HTTP protected by TLS.
- `www.google.com` is the hostname that must be translated into an IP address.
- `/` is the path of the requested resource. Here it means the site's root page.
- Port `443` is implied because it is the default port for HTTPS.

Before connecting to the network, the browser may also check its HTTP cache, service workers, security policies, and whether a previous connection to the same origin can be reused. Assuming a new network request is required, the browser must resolve the hostname.

## 2. DNS Resolves the Domain Name

Computers route packets using IP addresses, but people normally use domain names. The Domain Name System, or DNS, translates `www.google.com` into one or more IP addresses.

The browser and operating system first check local caches. They may examine:

1. The browser's DNS cache.
2. The operating system's DNS cache.
3. The local `hosts` file.
4. The configured recursive DNS resolver, often operated by a router, ISP, company, or public DNS provider.

If the recursive resolver has no cached answer, it follows the DNS hierarchy:

1. A root name server directs it to the `.com` top-level domain servers.
2. A `.com` server directs it to the authoritative name servers for `google.com`.
3. An authoritative server returns an address record, such as an IPv4 `A` record or an IPv6 `AAAA` record.

The reply has a time to live, or TTL, that controls how long it may be cached. Large services can return different addresses depending on the user's location, network conditions, and service availability. For example, DNS may direct a user in South America to infrastructure near that region rather than to a distant data center.

At the end of DNS resolution, the browser has a destination IP address.

## 3. TCP/IP Delivers Data to the Server

IP is responsible for addressing and routing packets across interconnected networks. Routers inspect the destination IP address and forward each packet toward the selected Google endpoint. Packets can cross the local network, an ISP, transit networks, and Google's network before reaching the destination.

For HTTP/1.1 and HTTP/2, the browser normally establishes a TCP connection to the server's IP address on port `443`. TCP provides a reliable, ordered byte stream. It begins with the three-way handshake:

```text
Browser  -> Server: SYN
Browser <-  Server: SYN-ACK
Browser  -> Server: ACK
```

The handshake synchronizes both endpoints and confirms that they are ready to communicate. TCP then handles sequence numbers, acknowledgements, retransmission of lost data, flow control, and congestion control.

The destination port identifies the service. Port `443` means the connection is intended for HTTPS. The client also uses a temporary source port, allowing the operating system to distinguish this connection from other active connections.

Modern browsers may use HTTP/3, which runs over QUIC and UDP instead of TCP. The TCP explanation remains important because HTTP/1.1 and HTTP/2 commonly use it and because this project specifically follows the classic TCP/IP request path.

## 4. Firewalls Check the Traffic

A firewall applies security rules to network traffic. Firewalls can exist on the user's computer, home router, cloud network, data-center edge, load balancer, and destination server.

For this request, an outbound firewall must allow the browser to connect to the destination IP on TCP port `443`. On the server side, an inbound firewall normally allows public HTTPS traffic while rejecting unexpected traffic, such as a direct connection to a private database port.

A rule might conceptually say:

```text
Allow established HTTPS traffic to the public service on TCP port 443.
Deny unauthorized direct traffic to internal application and database servers.
```

Firewalls reduce the exposed attack surface. Some systems also use web application firewalls, or WAFs, which inspect HTTP requests for application-level attacks after TLS has been terminated.

## 5. HTTPS/TLS Secures the Connection

HTTPS is HTTP carried through Transport Layer Security, or TLS. SSL is the older predecessor to TLS, so people still say "SSL certificate," although modern secure connections use TLS.

Before sending the HTTP request, the browser and server perform a TLS handshake. In a typical TLS 1.3 exchange:

1. The browser sends a `ClientHello` containing supported TLS versions, cipher suites, a key share, and the requested hostname through Server Name Indication, or SNI.
2. The server returns a `ServerHello`, selects cryptographic parameters, and sends its certificate chain.
3. The browser verifies that the certificate is valid, is signed by a trusted certificate authority, has not expired, and covers `www.google.com`.
4. Both sides derive shared session keys.
5. The handshake finishes, and application data is encrypted and authenticated.

TLS provides three central protections:

- **Confidentiality:** observers cannot read the HTTP request or response.
- **Integrity:** modification of the protected traffic is detected.
- **Authentication:** the certificate helps the browser confirm that it reached the legitimate domain.

After the handshake, the browser can safely send an encrypted request similar to:

```http
GET / HTTP/2
Host: www.google.com
User-Agent: ExampleBrowser/1.0
Accept: text/html
```

Network observers can still see some connection metadata, such as IP addresses and packet sizes, but they cannot normally read the encrypted HTTP content.

## 6. The Load Balancer Selects a Backend

A service as large as Google cannot depend on a single server. Incoming traffic reaches a load-balancing layer that distributes requests among many healthy backend servers.

A load balancer may operate at Layer 4 using IP addresses and ports or at Layer 7 using HTTP information such as the hostname, path, headers, or cookies. It can:

- Choose a nearby or lightly loaded backend.
- Stop sending traffic to an unhealthy server.
- Apply rate limits and security policies.
- Terminate TLS and create a new protected connection to an internal service.
- Route different paths to different applications.

Common distribution strategies include round robin, least connections, weighted routing, consistent hashing, and location-aware routing. For example, if one backend fails a health check, the load balancer removes it from rotation and directs new requests to healthy servers.

## 7. The Web Server Handles HTTP

The selected web server receives the HTTP request. A web server such as Nginx or Apache understands HTTP and is optimized for accepting connections and returning content.

For a static resource, the web server can answer directly. Examples include:

- An HTML file.
- A CSS stylesheet.
- A JavaScript bundle.
- A logo or other image.

The server adds an HTTP status code and response headers. A successful static response might begin like this:

```http
HTTP/2 200
Content-Type: text/html; charset=UTF-8
Cache-Control: private, max-age=0
```

If the page needs dynamic processing, the web server forwards the request to an application server.

## 8. The Application Server Runs the Business Logic

The application server executes the code that decides what the response should contain. It may be written in Python, Java, Go, JavaScript, C++, or another server-side language.

For a search page, application logic could:

- Read and validate request parameters.
- Determine language and regional settings.
- Check authentication or session information.
- Call internal services.
- Build a response model.
- Select or generate the HTML returned to the browser.

The web server and application server describe different responsibilities, but they do not have to be separate physical machines. In a modern distributed system, one request may pass through many application services before a response is complete.

## 9. The Application Requests Data from a Database

When the application needs persistent data, it queries a database or another storage system. Examples include account preferences, configuration, session data, or indexed content.

A simplified SQL query might look like:

```sql
SELECT language, theme
FROM user_preferences
WHERE user_id = 42;
```

The database parses the query, finds the relevant data, and returns a result to the application server. Production systems commonly use indexes, caches, read replicas, sharding, and connection pools to make this step faster and more reliable.

The database is normally isolated from the public internet. Only authorized internal services can reach it, often on a private network and through a restricted database port. The application should also use parameterized queries and limited database credentials to reduce security risks.

Not every request requires a database query. Frequently requested data may already be in an application cache, and static resources may be served without involving the application or database at all.

## 10. The Response Returns to the Browser

After the application server receives any necessary data, it generates a result and returns it to the web server. The response travels back through the load balancer, is encrypted by TLS, split into packets, and routed across the internet to the browser.

The browser decrypts the response and processes the HTTP status, headers, and body. For an HTML document, it then:

1. Parses HTML and builds the Document Object Model, or DOM.
2. Downloads referenced CSS, JavaScript, fonts, and images.
3. Parses CSS and builds the CSS Object Model, or CSSOM.
4. Combines the DOM and CSSOM into a render tree.
5. Calculates layout, paints pixels, and composites visual layers.
6. Executes JavaScript, which may change the page or make additional requests.

Caching headers can allow some resources to be reused on later visits. Connection reuse and TLS session resumption can also make subsequent requests faster.

## Conclusion

Pressing Enter starts a coordinated process across many layers:

```text
URL parsing
  -> DNS resolution
  -> IP routing and TCP connection to port 443
  -> firewall checks
  -> TLS encryption
  -> load balancing
  -> web server
  -> application server
  -> database
  -> encrypted response
  -> browser rendering
```

Each component has a distinct responsibility. DNS finds the destination, TCP/IP transports data, firewalls enforce access rules, TLS protects the communication, load balancers distribute work, web servers handle HTTP, application servers run business logic, and databases store persistent information. Together, these systems turn a short URL into an interactive web page.

## Explanation

This article follows the request from the browser to the infrastructure and back. It defines every topic required by Task 0 and uses examples such as DNS hierarchy lookups, the TCP handshake, port `443`, certificate validation, load-balancing strategies, HTTP headers, application logic, and an SQL query.

## Expected Output

After publishing, the expected output is a public English-language article that explains the complete HTTPS request lifecycle and includes the Task 1 diagram. Replace the Medium or LinkedIn placeholder near the top with the published URL.
