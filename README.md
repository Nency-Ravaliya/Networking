# Networking

# README.md

## Step 1: Your Laptop Gets an IP Address via DHCP

When your laptop connects to a network (Wi-Fi or Ethernet), it needs an IP address to communicate with other devices on the network and the internet. Here's how it happens:

1. DHCP Discovery – The Broadcast  
Your laptop sends a DHCP Discover message to request an IP address from a DHCP server (usually within the router):

- **Layer:** Application Layer (Layer 7, DHCP protocol)  
- **Message Content:** The message is a broadcast on the network, and it does not know the IP address of the DHCP server, so it’s sent to 255.255.255.255 (broadcast address).  
- **Switch Role:** The switch receives this broadcast message and forwards it to all connected devices, but the only device that responds is the router (because it acts as the DHCP server).  

2. DHCP Offer – IP Address Proposal  
The router (acting as the DHCP server) responds with a DHCP Offer message, proposing an IP address for your laptop:

- **Layer:** Application Layer (DHCP)  
- **Message Content:** This message contains the proposed IP address for your laptop (e.g., 192.168.1.50), the subnet mask (e.g., 255.255.255.0), and the default gateway (the router’s IP, e.g., 192.168.1.1).  
- **Switch Role:** The switch forwards the DHCP Offer only to your laptop based on its MAC address (because it keeps a table of devices connected to it).  

3. DHCP Request – Acceptance  
Your laptop responds to the router with a DHCP Request, saying it wants to use the proposed IP address:

- **Layer:** Application Layer (DHCP)  
- **Message Content:** The laptop sends this message to the router, confirming that it will use the IP address offered by the DHCP server.  

4. DHCP Acknowledgment – Confirmation  
The router (DHCP server) sends back a DHCP Acknowledgment to confirm the IP assignment:

- **Layer:** Application Layer (DHCP)  
- **Message Content:** The message confirms your laptop’s IP address, default gateway, subnet mask, and DNS server settings (often the router’s IP as well).  

Now your laptop has an IP address and knows how to route traffic through the router (the default gateway).

## Step 2: Connecting to Google.com

With an IP address assigned, your laptop can now communicate with devices outside of your local network. Let’s walk through the process of opening your browser and connecting to google.com.

### Layer 1-2: Physical and Data Link Layer (Switches, MAC Addresses)  
- **Switch Role:** When you send a request (to connect to google.com) from your laptop, the Ethernet frame containing the request is sent to the switch.  
- **Layer:** Data Link Layer (Layer 2)  
- **Message Content:** This frame contains the MAC address of your laptop and the MAC address of the next hop (which is usually the router in your home network).  
- **Switch Role:** The switch checks its MAC address table to determine which port to forward the frame to (in this case, to the router).  

### Layer 3: Network Layer (Routing, IP Addresses)  
- **Router Role:** The router receives the frame, unwraps it, and reads the IP packet inside, which contains the IP address of the destination server (google.com).  
- **Layer:** Network Layer (Layer 3)  
- **Message Content:** The IP packet contains:  
  - Source IP (your laptop’s IP, e.g., 192.168.1.50)  
  - Destination IP (one of Google’s IP addresses, e.g., 142.250.190.14)  
- **Router's IP:** The router has its own IP address on the local network (e.g., 192.168.1.1) and typically has a public-facing IP assigned by your Internet Service Provider (ISP).  
- **Routing:** The router forwards the packet toward google.com using its WAN interface. It knows where to send the packet next based on the routing table, which lists the paths to various destinations (Google is likely outside the local network, so the packet is forwarded to the ISP).  

### Layer 4: Transport Layer (TCP/UDP Ports)  
- **Transport Layer (TCP):**  
- **Layer:** Transport Layer (Layer 4)  
- **Message Content:** Inside the IP packet, the TCP segment includes:  
  - Source port (a random port number generated by your laptop, e.g., 49152)  
  - Destination port (usually port 80 for HTTP or 443 for HTTPS)  
- This segment establishes a connection between your laptop and Google’s web server using the TCP three-way handshake (SYN, SYN-ACK, ACK).  

### Layer 3 Again: Router and ISP  
- **Internet Routing (Hops):**  
  - The router forwards the packet to your ISP, which then passes it through a series of hops. Each hop is a router in the path between your network and google.com. Each of these routers forwards the packet closer to its final destination using IP addresses and routing tables.  
  - **Hops:** Each router along the path is a hop. You can use the traceroute command to see how many hops it takes to reach google.com. Each router along the way checks the IP address and forwards the packet based on the routing table.

## Step 3: Google.com Responds

Once your request reaches Google’s servers, they respond back, and the process works in reverse.

### Google's Web Server:
- Google’s server sends a response back to your laptop (e.g., the HTML code for google.com’s homepage).  
- **Layer 3 (IP):** The response includes Google’s IP address as the source IP and your laptop’s IP as the destination IP.  
- **Layer 4 (TCP):** The TCP segment includes the port numbers used to keep the connection open (e.g., 443 for HTTPS).  

### Router and Switch:
- As the response travels back, it passes through the same hops (routers) until it reaches your home router. The router checks the IP address and forwards the packet to your laptop via the switch.  
- The switch forwards the packet to your laptop using your MAC address.  

### Application Layer:
- Finally, your browser receives the data and renders google.com on your screen.

## Role of Components (Summary)

### Switch:
- Operates at Layer 2 and forwards frames based on MAC addresses within the local network (LAN). It connects devices like your laptop, router, and other devices.  
- Does not assign IP addresses but helps direct traffic within the LAN.  

### Bridge:
- Operates at Layer 2 as well and is used to connect two separate LAN segments, acting as a link between them.  

### Router:
- Operates at Layer 3 and forwards IP packets between different networks (your LAN and the internet). It has both a local IP (for LAN communication) and a public IP (for WAN/internet communication).  
- **DHCP Server:** Often acts as the DHCP server, assigning IP addresses to devices within the LAN.  

### Hop:
- A hop is any intermediary router between your home network and the destination (google.com). Each hop is a router forwarding the packet closer to its destination.

## What’s in a Request/Response?

### Request to Google.com:
- **Source IP:** Your laptop’s IP (e.g., 192.168.1.50)  
- **Destination IP:** Google’s server IP (e.g., 142.250.190.14)  
- **Transport Info:** Source and destination ports (e.g., 49152 → 443)  
- **Data:** Your browser’s HTTP request for google.com  

### Response from Google.com:
- **Source IP:** Google’s server IP  
- **Destination IP:** Your laptop’s IP  
- **Transport Info:** Google’s response data, such as HTML for the google.com page
