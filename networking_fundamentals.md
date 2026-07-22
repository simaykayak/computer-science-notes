# Networking Fundamentals

Networking is the technology that allows devices to exchange data with each other. When you visit a website, play a game, or send a message, these concepts are working in the background.

## IP (Internet Protocol)

An IP address is the identity of every device connected to the internet. It indicates where a packet came from and where it's going. It allows computers to find each other.   
192.168.1.15 or 142.250.184.206 (this could belong to one of Google's servers.)

* **IPv4**     
The most widely used system.   
192.168.0.1  
32 bits.   
Can produce approximately 4.3 billion addresses.   

* **IPv6**     
Developed when IPv4 addresses started running out.   
2001:0db8:85a3::8a2e:0370:7334   
128 bits.     
Can produce nearly unlimited addresses.     

IP addresses are also divided into private and public.   
For example, devices on a home network usually use private addresses like 192.168.x.x; when going out to the internet, these addresses are translated into a single public IP address through the modem/router (NAT - Network Address Translation).

## Port

Even though a device has a single IP address, dozens of applications can exchange data over the network at the same time (browser, email client, game, etc.).      
Port numbers are used so these applications don't get mixed up.

A simple analogy: if the IP address is the address of an apartment building, the port is the apartment number inside it. The packet reaches the right building (IP), then gets routed to the right apartment (port).

* **Commonly Used Ports**      
22 SSH      
53 DNS      
80 HTTP     
143 IMAP     
443 HTTPS       
3306 MySQL      
5432 PostgreSQL     

## DNS (Domain Name System)

People can't memorize IP addresses. That's why domain names are used.    
For example: google.com (the computer doesn't understand this.)      
First it asks DNS: "What is the IP address of google.com?"     
DNS answer: 142.250.184.206     
Then the connection is established.      

So: google.com -> DNS -> 142.250.184.206 -> Google Server     
DNS is like the phone book of the internet.

## TCP (Transmission Control Protocol)

* Connection-based: before data is sent, a "handshake" takes place between sender and receiver (three-way handshake: SYN – SYN/ACK – ACK).
* Reliable: if a packet is lost, it's resent.
* Order of packets is preserved; on the receiving end, data is reassembled in the order it was sent.
* TCP is slower than UDP and carries more overhead.
* Use cases: web browsing (HTTP/HTTPS), email, file transfer — anywhere data needs to arrive complete and in the correct order.

## UDP (User Datagram Protocol)

* Connectionless: no handshake, data is sent directly.
* Doesn't care about lost packets; the sender just says "I sent it, done" without guaranteeing it reached the receiver.
* Packet order is not preserved.
* This makes it much faster and lower latency.
* Use cases: live video/audio streaming, online games, VoIP — where losing a few packets matters less than experiencing delay (for example, if a game's real-time position update arrives late, it's already useless, so there's no point resending it).

## How Does Packet Structure Work?

Large files aren't sent over the internet as a single piece. They are split into small packets.     
For example: a 100 MB file -> Packet 1, Packet 2....     
Each packet roughly contains: source IP, destination IP, source port, destination port, data, packet number, error-check information (checksum).   

Packets can take different routes across the internet.   
For example: A -> Router 1 -> Router 2 -> B     
Another packet: A -> Router 5 -> Router 8 -> B      
They are reassembled in the correct order at the destination (especially in TCP connections).    

## Ping

Ping tests whether there is a connection between two devices.   
Command: ping google.com     
Example output: Reply from 142.250.184.206       time=18 ms

Information learned:
* Is the server running?
* Is there an internet connection?
* What is the latency (in ms)?
* Is there packet loss?

## Traceroute

Shows the routers a packet passes through on its way to the destination.    
Windows: tracert google.com.    
Linux/macOS: traceroute google.com.    
Example: 1 Modem, 2 ISP, 3 Istanbul, 4 Frankfurt, 5 Google     
This way, you can understand at which point delay or a connection problem occurs.     

## nslookup

Performs a DNS query.     
Command: nslookup google.com      
Example: Name: google.com       Address: 142.250.184.206.     
With this you can:   
* Find out the IP address of a domain name.
* Test whether DNS is working correctly.
* Compare results coming from different DNS servers.

## How the Concepts Work Together

When you type www.google.com into a browser, the following steps happen in the background:
1. DNS translates the domain name www.google.com into an IP address.
2. The computer tries to connect to the target server's IP address.
3. A reliable connection is established with TCP. (For HTTPS, port 443 is generally used.)
4. The request and response are carried over the internet in small packets.
5. The server receives the packet and sends the web page back, also in packets.
6. The browser reassembles these packets and displays the page on screen.