# Networking

# README.md

## Step 1: Your Laptop Gets an IP Address via DHCP

When your laptop connects to a network (Wi-Fi or Ethernet), it needs an IP address to communicate with other devices on the network and the internet. Here's how it happens:

### 1. DHCP Discovery – The Broadcast
- **Layer:** Application Layer (Layer 7, DHCP protocol)
- **Message Content:** Your laptop sends a DHCP Discover message as a broadcast on the network to request an IP address from a DHCP server (usually within the router).
- **Switch Role:** The switch receives this broadcast and forwards it to all devices, but the router (acting as the DHCP server) responds.

### 2. DHCP Offer – IP Address Proposal
- **Layer:** Application Layer (DHCP)
- **Message Content:** The router responds with a DHCP Offer message proposing an IP address for your laptop, including the subnet mask and default gateway.
- **Switch Role:** The switch forwards the DHCP Offer to your laptop based on its MAC address.

### 3. DHCP Request – Acceptance
- **Layer:** Application Layer (DHCP)
- **Message Content:** Your laptop responds with a DHCP Request message, indicating it accepts the proposed IP address.

### 4. DHCP Acknowledgment – Confirmation
- **Layer:** Application Layer (DHCP)
- **Message Content:** The router confirms the IP assignment, providing the laptop with the default gateway, subnet mask, and DNS server settings.

Now your laptop has an IP address and is ready to route traffic through the router (default gateway).

---

## Step 2: Connecting to Google.com

With an IP address assigned, your laptop can communicate with devices outside the local network. Here's the process of opening your browser and connecting to google.com.

### Layer 1-2: Physical and Data Link Layer (Switches, MAC Addresses)
- **Switch Role:** The Ethernet frame containing the request is sent to the switch.
- **Layer:** Data Link Layer (Layer 2)
- **Message Content:** The frame contains the MAC addresses of your laptop and the router.

### Layer 3: Network Layer (Routing, IP Addresses)
- **Router Role:** The router reads the IP packet inside the frame.
- **Layer:** Network Layer (Layer 3)
- **Message Content:** The packet contains:
  - Source IP (your laptop's IP, e.g., 192.168.1.50)
  - Destination IP (Google’s IP, e.g., 142.250.190.14)
  - The router forwards the packet towards google.com based on its routing table.

### Layer 4: Transport Layer (TCP/UDP Ports)
- **Layer:** Transport Layer (Layer 4)
- **Message Content:** The TCP segment includes the source port and destination port, establishing a connection using the TCP three-way handshake.

### Layer 3 Again: Router and ISP
- **Internet Routing (Hops):** The packet is forwarded to your ISP and passes through several hops (routers) toward Google.

---

## Step 3: Google.com Responds

When Google’s server receives the request, it sends a response back, following the same path in reverse.

### Google's Web Server:
- Google sends back the response (e.g., HTML for google.com).
- **Layer 3 (IP):** The source IP is Google’s, and the destination IP is your laptop's.
- **Layer 4 (TCP):** The TCP segment keeps the connection open.

### Router and Switch:
- The response packet is forwarded back to your laptop through the same hops.

### Application Layer:
- Your browser receives the data and renders google.com.

---

## Role of Network Components

### Switch:
- Operates at Layer 2, forwarding frames based on MAC addresses within the LAN.
- Directs traffic locally but does not assign IP addresses.

### Bridge:
- Operates at Layer 2 and connects two separate LAN segments.

### Router:
- Operates at Layer 3, forwarding IP packets between networks.
- Acts as the DHCP server and assigns IP addresses to devices on the LAN.

### Hop:
- A hop is any intermediary router between your home network and the destination.

---

## What’s in a Request/Response?

### Request to Google.com:
- **Source IP:** Your laptop's IP (e.g., 192.168.1.50)
- **Destination IP:** Google’s server IP (e.g., 142.250.190.14)
- **Transport Info:** Source and destination ports (e.g., 49152 → 443)
- **Data:** HTTP request for google.com.

### Response from Google.com:
- **Source IP:** Google’s IP
- **Destination IP:** Your laptop's IP
- **Transport Info:** Google’s response data (HTML for google.com).
