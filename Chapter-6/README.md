# The Link Layer
### Cyclic Redundancy Check (CRC)
CRC is used to detect errors. 
- CRC is based on binary divisors.
- There are total m + r bits. Say the polynomial function is (x^4 + x^3 + 1).
r is also known as CRC bits.
- It's impossible to detect the errors only from the pure bits. As a result,
we also send some redundant bits.
- Now one might ask what will be the number of redundant bits. The easiest
answer is to the highest number of degrees it has.
- In this case, this number is 4.

- Say, the data is 1010101010. So we add 4 more 0 to this.
- It becomes, 1010101010`0000`. Now these last 4 bits will be replaced by the
divisor.
- One might ask now, how one can find the divisor.
- We have to find the coefficient first. In this example, the coefficients
are (1.x^4+1.x^3+0.x^2+0.x^1+1.x^0)
- So the coefficient becomes 11001. Say the divisor is given in this binary
form. So to get the redundant bits from the divisor, we add n-1 bits.
- Now let's do the calculation.

1. `10101`010100000 / 11001 = `01100`. Now we borrow 1 from the right. As
calculations have to be done from a leading 1.
2. `011000`/11001 = `00001`. Now we borrow 4. (`101010`10100000)
3. `11010`/11001 = `00011`. Now we borrow 3 more.(`1010101010`0000).
4. `11000`/11001 = `00001`(`1010101010000`0).
5. 00`0010`. Since there been less than 4-bits left, we consider the last
4-bits and replace with the redundant bits.
6. 1010101010`0010`. These whole 14 bits will be sent to the receiver.

So, when the receiver receives the message, it does the same process:
1. `10101`010100010 / 11001 = `01100`. We borrow 1-bit from the right. (`101010`10100010)
2. 11000 / 11001 = 00001. We borrow 4.
3. 11010 / 11001 = 00011. We borrow 3 from a right.
4. 11001/11001 = 00000. As the remaining bit in the right is also 0. We can
conclude all the bits being 0.
5. Hence, the data is not corrupted.
6. If any of the bits in the way, changed the result would not have been all 0.

### MAC address
1. **Definition**: A MAC address serves as a **physical address** for a network 
device, operating at the **Data Link Layer**. It uniquely identifies a specific 
device on a network.

2. **Format**: MAC addresses consist of **12 hexadecimal digits**, separated 
every two digits by a colon or hyphen. For example, a MAC address like 
`2c549188c9e3` can be displayed as `2C:54:91:88:C9:E3` or `2c-54-91-88-c9-e3`.

3. **Network Adapter**: MAC addresses work in conjunction with the **Network 
Interface Controller (NIC)** in your device. This card enables wireless 
connectivity to the internet via Wi-Fi, Ethernet, or Bluetooth. Each device 
has its own unique MAC address.

4. **Functionality**: When data packets from the internet reach your router, 
the router needs to direct them to the correct device on its network. It 
accomplishes this by using MAC addresses. Your router assigns a **private IP 
address** to each network-connected device based on its MAC address. Unlike 
the **public IP address** assigned by your internet service provider (ISP), 
which is used for external communication, the private IP address is used 
internally within your local network. The router keeps track of outbound data 
requests and associates the correct private IP with incoming data packets, 
forwarding them to the device whose MAC address matches that private IP. 
Devices can have multiple MAC addresses if they can connect to the internet 
through different interfaces (e.g., Wi-Fi and Ethernet) on the same device.

### ARP && RARP
1. **Address Resolution Protocol (ARP)**:
    - **Purpose**: ARP is a networking protocol used to map an **Internet Protocol 
   (IP) address** to a **physical (MAC) address**. It operates at the **data link 
   layer** of the OSI model.
    - **Functionality**:
        - When a device wants to communicate with another device on the same local 
      network, it needs to know the recipient's MAC address.
        - The sender broadcasts an ARP request containing the target IP address.
        - The device with the matching IP address responds with its MAC address.
        - The sender then caches this mapping in its ARP table for future use.
    - **Use Case**: ARP is essential for local network communication, allowing 
   devices to find each other based on IP addresses.

2. **Reverse Address Resolution Protocol (RARP)**:
    - **Purpose**: RARP is the reverse of ARP. It maps a **MAC address to an IP 
   address**.
    - **History and Use Case**:
        - RARP was developed in the early days of computer networking to provide 
      IP addresses to **diskless workstations** or devices that couldn't store 
      their own IP addresses.
        - Diskless workstations would broadcast their MAC address and request an 
      IP address.
        - A RARP server on the network would respond with the corresponding IP 
      address.
    - **Limitations and Replacement**:
        - RARP has largely been replaced by newer protocols like **DHCP (Dynamic 
      Host Configuration Protocol)**, which offers more flexibility in dynamically
      assigning IP addresses.
        - However, RARP is still used in specialized contexts, such as booting 
      embedded systems and configuring network devices with pre-assigned IP 
      addresses.
    - **Protocol Specification**: RARP is specified in **RFC 903**.

### Man-in-the-middle attack
A **Man-in-the-Middle (MITM) attack** is a type of active attack where an attacker 
intercepts communications between two parties on a network. The attacker secretly relays 
and possibly modifies the data exchanged between the victims, making them believe they are 
communicating directly with each other. One common technique for MITM attacks is **ARP 
spoofing** (also known as **ARP poisoning**). Let's explore ARP spoofing in more detail:

**ARP Spoofing (ARP Poisoning)**:
   - **Definition**: ARP stands for **Address Resolution Protocol**. It is used for 
   resolving **IP addresses to machine MAC addresses** within a local network.
   - **How It Works**:
      - All devices in a network broadcast ARP queries to find out the MAC addresses of 
     other machines.
      - In ARP spoofing, the attacker sends forged ARP packets to force data to be sent to 
     their own machine.
      - By doing this, the attacker becomes the **man-in-the-middle**, intercepting traffic
     between the victims.
      - The attacker pretends to be both the victim and the router, redirecting requests 
     and responses through their system.
   - **Impact**:
      - ARP spoofing allows the attacker to capture sensitive information, modify data, 
     or perform other malicious actions.
      - It can lead to unauthorized access, session hijacking, and data theft.
   - **Mitigation**:
      - To defend against ARP spoofing, network administrators can:
         - Implement **ARP monitoring tools** to detect suspicious ARP activity.
         - Use **static ARP entries** to prevent dynamic ARP poisoning.
         - Deploy **network segmentation** to limit the impact of ARP spoofing.

### Ethernet frame structure
The structure of an Ethernet frame is defined by the IEEE 802.3 standard and is essential 
for the functioning of Ethernet networks. Here's a breakdown of the typical Ethernet frame 
structure:

1. **Preamble**: A 7-byte sequence used to synchronize the receiver before actual data is 
sent. It consists of alternating 1s and 0s, ending with two 1s.

2. **Start Frame Delimiter (SFD)**: A 1-byte field that signals the beginning of the frame. 
It is set to the binary pattern `10101011`.

3. **Destination MAC Address**: A 6-byte field containing the MAC address of the destination 
device.

4. **Source MAC Address**: A 6-byte field containing the MAC address of the source device.

5. **802.1Q Tag (optional)**: A 4-byte field used for VLAN tagging.

6. **Type/Length**: A 2-byte field indicating either the type of payload for Ethernet II 
framing or the length of the payload for IEEE 802.3 framing.

7. **Payload**: The data being transported, which can be a maximum of 1500 bytes (the 
maximum transmission unit, or MTU). If the data is less than the minimum frame size, padding 
bytes are added to meet the minimum frame length requirement.

8. **Frame Check Sequence (FCS)**: A 4-byte field containing a cyclic redundancy check (CRC) 
value used for error checking.

9. **Interpacket Gap (IPG)**: A period of silence on the network that allows devices to 
recover and prepare for the next frame.

### Packet Datagram Units (PDU). Construction of a TCP/IP ethernet data packet.
The **Packet Datagram Unit (PDU)** is a term used in networking to refer to a single 
unit of data at different layers of the TCP/IP protocol suite. Each layer of the TCP/IP 
model has its own PDU format and name. When constructing a TCP/IP Ethernet data packet, 
encapsulation occurs, where each layer adds its own header (and sometimes a footer) to the 
data from the upper layer. Here's how a TCP/IP Ethernet data packet is constructed:

1. **Application Layer**:
    - The data generated by an application is considered the PDU at this layer.
    - **PDU Name**: Data

2. **Transport Layer**:
    - The application layer data is encapsulated within a **TCP segment** or **UDP datagram**, 
   depending on the protocol used.
    - **PDU Name**: Segment (TCP) or Datagram (UDP)
    - **Header Information**: Includes source and destination port numbers, sequence and 
   acknowledgment numbers (TCP), and other control information.

3. **Network Layer**:
    - The transport layer PDU is encapsulated within an **IP packet**.
    - **PDU Name**: Packet
    - **Header Information**: Contains source and destination IP addresses, version, length, 
   fragmentation, and other flags.

4. **Link Layer**:
    - The network layer PDU is encapsulated within an **Ethernet frame**.
    - **PDU Name**: Frame
    - **Header Information**: Includes the destination and source MAC addresses, and type/
   length field.
    - **Footer**: Contains a Frame Check Sequence (FCS) for error checking.

5. **Physical Layer**:
    - The link layer frame is converted into a bitstream for transmission over the physical 
   medium.
    - **PDU Name**: Bits

Here's a simplified representation of the encapsulation process in a TCP/IP Ethernet data 
packet:

```
+-----------------+
| Application Data |
+-----------------+
        |
        v
+-----------------+
| TCP/UDP Header  |
+-----------------+
| Application Data |
+-----------------+
        |
        v
+-----------------+
| IP Header       |
+-----------------+
| TCP/UDP Header  |
+-----------------+
| Application Data |
+-----------------+
        |
        v
+-----------------+
| Ethernet Header |
+-----------------+
| IP Header       |
+-----------------+
| TCP/UDP Header  |
+-----------------+
| Application Data |
+-----------------+
| Ethernet Footer |
+-----------------+
        |
        v
+-----------------+
| Physical Bits   |
+-----------------+
```

### Switching vs. Routing
**Switching:**

Switching is a process that occurs at the Data Link Layer (Layer 2) of the OSI model. It's 
responsible for forwarding frames (packets of data) within a single network or a small 
group of networks. 

Here's how switching works:

1. A device sends a frame to a switch.
2. The switch examines the MAC address of the frame and determines the destination device.
3. The switch forwards the frame to the destination device.

**Routing:**

Routing is a process that occurs at the Network Layer (Layer 3) of the OSI model. It's 
responsible for forwarding packets (datagrams) between different networks or subnets.

Here's how routing works:

1. A device sends a packet to a router.
2. The router examines the IP address of the packet and determines the destination network 
or subnet.
3. The router forwards the packet to the next hop (router or gateway) on the path to the 
destination.

**Key differences:**

1. **Scope:** Switching operates within a single network or a small group of networks, while
routing operates between different networks or subnets.
2. **Layer:** Switching occurs at the Data Link Layer (Layer 2), while routing occurs at the
Network Layer (Layer 3).
3. **Addressing:** Switching uses MAC addresses, while routing uses IP addresses.

### Hub, Switch and Router
1. **Hub**:
   - **Layer**: Physical Layer (Layer 1).
   - **Function**: A hub is a basic networking device that connects multiple computers or 
   other network devices together. It broadcasts incoming data packets to all ports, 
   regardless of the destination.
   - **Intelligence**: Low. It does not filter data or have any understanding of the data 
   it sends.

2. **Switch**:
   - **Layer**: Data Link Layer (Layer 2).
   - **Function**: A switch is more advanced than a hub. It connects devices and, unlike a 
   hub, can intelligently determine the destination of the data packets it receives and send 
   them to the correct device directly.
   - **Intelligence**: Medium. It uses MAC addresses to process and forward data to the 
   correct destination.

3. **Router**:
   - **Layer**: Network Layer (Layer 3).
   - **Function**: A router is the most complex of the three. It is used to connect multiple 
   networks together and route data packets between them.
   - **Intelligence**: High. It determines the best path for data packets based on IP 
   addresses. It can manage traffic, perform network address translation, and implement 
   security protocols.

### ICMP
ICMP (Internet Control Message Protocol) is a crucial part of the Internet Protocol (IP) 
suite, and it plays a vital role in troubleshooting and debugging network issues.

**What does ICMP do?**

ICMP is used to:

1. **Report errors**: ICMP is used to report errors that occur during IP packet transmission.
For example, if a packet is too large to be sent over a network, ICMP will send an error message back to the sender.
2. **Provide diagnostic information**: ICMP is used to provide diagnostic information about 
network issues. For example, if a packet is lost or corrupted during transmission, ICMP will 
send an error message to the sender.
3. **Test network connectivity**: ICMP is used to test network connectivity between devices. 
For example, the "ping" command uses ICMP to send a packet to a remote device and measure 
the time it takes for the packet to return.

**ICMP in action**

Here's an example of how ICMP works:

1. You use the "ping" command to send a packet to a remote device.
2. The packet is sent to the remote device, but it's lost or corrupted during transmission.
3. The router or device that received the packet sends an ICMP "Destination Unreachable" 
message back to your device.
4. Your device receives the ICMP message and displays an error message indicating that the 
packet was not delivered.