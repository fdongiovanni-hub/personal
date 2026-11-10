# Networking

## ISO OSI model

### Layer 1: Physical Layer

physical connection between devices; this includes the medium, such as a wire, and the definition of the binary digits 0 and 1.

### Layer 2: Data Link Layer

represents the protocol that enables data transfer between nodes on the same network segment. (MAC addresses)

### Layer 3: Network Layer
is concerned with sending data between different networks. In more technical terms, the network layer handles logical addressing and routing, i.e., finding a path to transfer the network packets between the diverse networks.

Examples of the network layer include Internet Protocol (IP), Internet Control Message Protocol (ICMP), and Virtual Private Network (VPN) protocols such as IPSec and SSL/TLS VPN.

When your home router receives the packet from your computer, it strips off your local LAN's Layer 2 header (destroying the trace of your computer's MAC address). It looks at the Destination IP (Layer 3), realizes the packet is bound for the internet, and prepares to send it out of its WAN (external) port.
- FTTH / Ethernet Connections: If your ISP provides a direct Ethernet handoff (common with some fiber setups), a standard Ethernet header is used. The Source MAC becomes your router's WAN port, and the Destination MAC becomes the ISP's router interface.
- PPPoE / DSL Connections: If your connection uses PPPoE (Point-to-Point Protocol over Ethernet), it creates a direct Layer 2 virtual tunnel between your router and the ISP's Broadband Remote Access Server (BRAS). Inside this tunnel, standard MAC-to-MAC routing isn't used to find the path; the session ID handles the delivery.

### Layer 4: Transport Layer
Layer 4 doesn't care how a packet travels across the city. Its only job is to deliver data directly to a specific program running on your computer (like your browser, Discord, or a game) and decide how that data should be behaved.

Uses ports:
- Your web browser connects using Port 80 (HTTP) or 443 (HTTPS).
- Your SSH terminal connects using Port 22.

Two ways to move data:
- TCP (reliable): Establishes a connection first (3-way handshake). Checks if every single piece arrives safely.
- UDP (fast): fires the data into the network. Doesn't check if it arrives.

- Segmentation (Chopping data): You cannot send a 4GB movie over the network in one massive block. Layer 4 chops that movie into tiny, manageable pieces (segments). On the receiving end, Layer 4 puts them back together in the correct order.
- Flow Control (Speed matching): If a powerful server sends data faster than a device can process it, Layer 4 on that device tells the server to slow down. This prevents the device from dropping packets due to overload.

### Layer 5: Session Layer

The Transport Layer (Layer 4) is only responsible for moving raw data segments from one device to another. It does not understand the concept of an ongoing conversation. Layer 5 fixes this by creating and managing a formal session between the two applications.
When you log into a service, Layer 5 negotiates the rules of the communication, coordinates which side speaks first, and maintains the connection open. Its most critical function is data synchronization through checkpoints. If a large 10GB file transfer fails at 7GB due to a temporary network drop, Layer 5 relies on these internal markers to resume the transfer from the last valid checkpoint, preventing the application from restarting the entire process from scratch.

### Layer 6: Presentation Layer

Computers do not all store or process data the same way. A Linux server, a Windows PC, and a Mac might use different internal architectures. Layer 6 acts as a universal translator, formatting the raw data syntax so that both the network and the application can understand it.
This layer handles three major processes:
- Data Encoding: It translates raw bits into standardized formats. For text, it uses character sets like ASCII or UTF-8. For images and multimedia, it defines structures like JPEG, PNG, or MP4. In email systems, it utilizes MIME to translate complex binary files (like PDF attachments) into 7-bit ASCII text so they can travel across legacy mail systems safely.
- Compression: It shrinks the data size before transmission to optimize bandwidth, which is then decompressed on the receiving side.
- Encryption: It processes cryptographic functions to secure the data structure before passing it down the stack.


### Layer 7: Application Layer

The Application Layer is the top layer of the OSI model and acts as the interface between the human user and the network stack. It is important to note that Layer 7 is not the software application itself (like Google Chrome or Outlook), but rather the specific network protocols that those software programs call upon to perform operations.
When you interact with a software application, it communicates using standard Layer 7 protocols designed for precise tasks:
- Web Browsing: The browser calls HTTP or HTTPS to request and structure web page content.
- Domain Resolution: The operating system uses DNS to look up the IP address associated with a human-readable domain name.
- Email Handling: Mail clients use SMTP to send messages and IMAP or POP3 to retrieve them from servers.
- File Management: Programs use FTP or SFTP to move files securely across remote systems.

## TCP/IP model
- Application Layer: The OSI model application, presentation and session layers, i.e., layers 5, 6, and 7, are grouped into the application layer in the TCP/IP model.
- Transport Layer: This is layer 4.
- Internet Layer: This is layer 3. The OSI model’s network layer is called the Internet layer in the TCP/IP model.
- Link Layer: This is layer 2.

## Incapsulation
- Application data: It all starts when the user inputs the data they want to send into the application. For example, you write an email or an instant message and hit the send button. The application formats this data and starts sending it according to the application protocol used, using the layer below it, the transport layer.
- Transport protocol segment or datagram: The transport layer, such as TCP or UDP, adds the proper header information and creates the TCP segment (or UDP datagram). This segment is sent to the layer below it, the network layer.
- Network packet: The network layer, i.e. the Internet layer, adds an IP header to the received TCP segment or UDP datagram. Then, this IP packet is sent to the layer below it, the data link layer.
- Data link frame: The Ethernet or WiFi receives the IP packet and adds the proper header and trailer, creating a frame.

## IP Addresses
IP addresses are categorized as either public or private. Private addresses cannot directly access or be reached by the public internet. For a private device to browse the internet, a router equipped with a public IP must perform Network Address Translation (NAT). RFC 1918 defines three specific private address ranges that should be memorized:
• 10.0.0.0 - 10.255.255.255 (10/8)
• 172.16.0.0 - 172.31.255.255 (172.16/12)
• 192.168.0.0 - 192.168.255.255 (192.168/16)


## Interaction with Network Services: Telnet & Banner Grabbing

TELNET is a legacy protocol used to open plain, unencrypted TCP connections to specific service ports. While replaced by SSH for secure administration, it remains highly efficient for diagnostics and **Banner Grabbing** (discovering specific service names and version numbers, such as `lighttpd/1.4.63`, to look up known vulnerabilities).

### Laboratory Services Summary
- **Echo (Port 7):** A simple diagnostic server that returns any sent text back to the client.
- **Daytime (Port 13):** Instantly replies with the current system date/time and drops the connection.
- **Web HTTP (Port 80):** Serves web application traffic. Interacting with it requires manual HTTP/1.1 request formatting at the Application Layer (e.g., typing `GET / HTTP/1.1` followed by a custom `Host:` header).

### Command Tool Comparison
While **Telnet** and **Netcat (`nc`)** open a raw, manual connection tube where you must type the raw HTTP protocol lines by hand before the timeout triggers, **`curl`** acts as an automated utility that handles the protocol formatting instantly in the background to fetch the web page content directly.
