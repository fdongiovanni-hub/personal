# Networking

## ISO OSI model

### Layer 1: Physical Layer

physical connection between devices; this includes the medium, such as a wire, and the definition of the binary digits 0 and 1.

### Layer 2: Data Link Layer

represents the protocol that enables data transfer between nodes on the same network segment.

### Layer 3: Network Layer
is concerned with sending data between different networks. In more technical terms, the network layer handles logical addressing and routing, i.e., finding a path to transfer the network packets between the diverse networks.

Examples of the network layer include Internet Protocol (IP), Internet Control Message Protocol (ICMP), and Virtual Private Network (VPN) protocols such as IPSec and SSL/TLS VPN.

When your home router receives the packet from your computer, it strips off your local LAN's Layer 2 header (destroying the trace of your computer's MAC address). It looks at the Destination IP (Layer 3), realizes the packet is bound for the internet, and prepares to send it out of its WAN (external) port.
- FTTH / Ethernet Connections: If your ISP provides a direct Ethernet handoff (common with some fiber setups), a standard Ethernet header is used. The Source MAC becomes your router's WAN port, and the Destination MAC becomes the ISP's router interface.
- PPPoE / DSL Connections: If your connection uses PPPoE (Point-to-Point Protocol over Ethernet), it creates a direct Layer 2 virtual tunnel between your router and the ISP's Broadband Remote Access Server (BRAS). Inside this tunnel, standard MAC-to-MAC routing isn't used to find the path; the session ID handles the delivery.