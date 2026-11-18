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