# What Happens When You Type `google.com` in Your Browser and Press Enter?

This directory explains the complete lifecycle of an HTTPS request, from entering a URL to rendering the returned page.

## Files

| File | Description |
| --- | --- |
| `0-blog_post.md` | Detailed English-language article covering the complete web request flow. |
| `1-what_happen_when_diagram.md` | Diagram answer, editable Mermaid source, explanations, and expected output. |
| `request_flow.svg` | SVG diagram used by both task files. |
| `README.md` | Task summary, answers, explanations, expected outputs, and commit commands. |

## Task 0: "What happens when..."

### Answer

The complete answer is in [`0-blog_post.md`](./0-blog_post.md). It covers:

- DNS request and hostname resolution.
- TCP/IP and the TCP three-way handshake.
- Firewalls and port `443`.
- HTTPS, TLS, and certificate validation.
- Load-balancing responsibilities and strategies.
- Static content from a web server.
- Dynamic processing by an application server.
- Persistent data requested from a database.
- The response path and browser rendering.

### Explanation

The article follows one request in chronological order. It explains what each infrastructure component does, why it is necessary, and how it communicates with the next component. It also notes that HTTP/3 can use QUIC over UDP, while the classic HTTP/1.1 and HTTP/2 path uses TCP.

### Expected Output

A public English-language blog post containing the required concepts and the request-flow diagram. The article is ready to publish, but its Medium or LinkedIn URL must be added after publication from the repository owner's account.

### Git Commit Message

```bash
git add what_happens_when_your_type_google_com_in_your_browser_and_press_enter/0-blog_post.md
git commit -m 'added Task 0: "What happens when..." blog post'
```

## Task 1: "Everything's better with a pretty diagram"

### Answer

The diagram and its explanation are in [`1-what_happen_when_diagram.md`](./1-what_happen_when_diagram.md). The image file is [`request_flow.svg`](./request_flow.svg).

It shows:

- DNS resolving `www.google.com`.
- TCP connecting to the server IP on port `443`.
- Firewall filtering.
- TLS encryption.
- Load-balancer distribution.
- Web-server handling.
- Application-server processing.
- Database access and return data.
- The encrypted response returning to the browser.

### Explanation

The solid arrows follow the request toward the backend. Dashed arrows show data and the generated response returning toward the browser. Colors separate networking, security, routing, processing, and storage responsibilities.

### Expected Output

GitHub renders the embedded SVG in both task files. The direct raw image URL becomes available after the files are pushed to the `main` branch:

```text
https://raw.githubusercontent.com/JGonzalez-7/holbertonschool-network/main/what_happens_when_your_type_google_com_in_your_browser_and_press_enter/request_flow.svg
```

### Git Commit Message

```bash
git add what_happens_when_your_type_google_com_in_your_browser_and_press_enter/1-what_happen_when_diagram.md \
  what_happens_when_your_type_google_com_in_your_browser_and_press_enter/request_flow.svg
git commit -m "added Task 1: \"Everything's better with a pretty diagram\""
```

## Documentation Commit

To commit the task files and both README updates together:

```bash
git add README.md what_happens_when_your_type_google_com_in_your_browser_and_press_enter
git commit -m "added web request lifecycle article and diagram"
```
