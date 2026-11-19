## Web Browser
TCP port 80 is the default for plain HTTP, but ususally websites automatically redirect to HTTPS (TCP port 443). Modern sites also support HTTP/3 (QUIC protocol, UDP port 443).

QUIC is a transport protocol that combines TCP and TLS. faster and more reliable connections than traditional TCP+TLS.

### Developer Tools
Ctrl + Shift + I on Windows and Linux, or Option + Command + I on macOS, in Firefox, Chrome, Edge, or most Chromium-based browsers

#### Network Tab
request and responses headers:
- Server: name and software version
- X-Powered-By: language or framework
- Content-Security-Policy: white list of secure sources from which the server can load files

timing data, status codes, and cookies

#### Console Tab
Allows to execute JavaScript snippets in the page contexts, view errors and interact with DOM (Document Object Model, tree structured representation of a web page that the browser creates)

#### Sources Tab
browse JavaScript, CSS, HTML loaded by the page. Most practical reconnaissance techniques available, because can surface information invisible in rendered page.

#### Application Tab
cookies, local storage, session storage

#### Security Tab
provides certificate details. SANs (Subject Alternative Names) may reveal additional subdomains or related domains.

### Browser Extensions
- FoxyProxy: allows you to switch between proxies such as Burp Suite, ZAP, and SOCKS5 tunnels

- User-Agent Switcher and Manager: changes the User-Agent string to emulate different browsers, operating systems, or devices. You can present as mobile Safari or an older browser version to discover mobile-specific endpoints or version-specific behaviour.

- Wappalyzer (or BuiltWith Technology Profiler): automatically identifies technologies used on the site, including CMS platforms, web servers, JavaScript frameworks, analytics tools, CDNs, and databases.
(WhatRuns and Library Detector for Chrome/Firefox are also similar)

## PING
uses ICMP (Internet Control Message Protocol, protocol used by devices to exchange check messages), sends a ICMP Echo Request packet (type 8), and if the target is permitted to answer, it sends back an ICMP Echo Reply (Type 0). It's a very light and fast exchange.
The reply's payload has to be the same as the request payload.

*some code numbers inside the header of the ICMP packet to tell the exact objective of the message: type 8: echo request (usually firewalls block those), type 0: echo reply, type 3: destination unreachable, type 11: time exceeded*

```ping -4 -c 5 10.114.162.127```
- -c flag: specify the number of packets to send
- -4: specify IP version

in the output it specifies "TTL" (Time To Live) field, which is the maximum number of routers (hops) a packet can pass through before being dropped (usually linux sets TTL=64, windows sets TTL=128)

If byte size -s not specified, it sends regular 56 bytes payload added to the 8 bytes of the ICMP header (and added to the 20 bytes of the IPv4 header).

## TRACEROUTE
discovers number and IP addresses of routers (hops).
routes packets take is not fixed, many routers use dynamic routing protocols (BGP or OSPF).
Basically works sending many packets with incrementally increasing TTL values starting at 1. The first router has to drop the packet and respond (showing its own IP), the next packet (TTL=2) reaches the second router etc. Some secure routers are configured not to send ICMP Time-to-Live Exceeded messages.

```traceroute -T 10.114.162.127```
- -T flag: TCP-based tracing instead of UDP
- -I flag: ICMP-based tracing

## TELNET
Teletype Network protocol, developed to communicate with a remote system via a command-line interface (default port 23).
Operates over TCP, and you can use it to connect to any TCP port and observe the server's response (banner grabbing).
Generally netcat and curl are preferred because they provide similar functionality with more flexibility.

## NETCAT
versatile networking utility that supports TCP and UDP protocols, can be used as client or as a server that listens. useful for banner grabbing, port probing, simple file transfers, and basic client-server communication.

```nc 10.114.162.28 80```
```GET / HTTP/1.1```
```host: netcat```
opens a direct TCP connection towards the IP address on the specified door. with GET sends the manual raw HTTP request asking the main resource (/) to the server, specifing 1.1 HTTP version.
host is a mandatory header of the HTTP/1.1 protocol.

Using port 21  will typically produce an immediate banner showing the FTP server software (so HTTP is not working!).
Connecting to an SMTP server on port 25 produces a banner identifying the mail server (if present).

```nc -vnlp 1234```
start listening on port 1234