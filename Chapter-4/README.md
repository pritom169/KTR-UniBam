# The Network Layer: The Data Plane
### Data Plane Vs Control Plane
- **Focus and Functions**:
  - **Control Plane:** Manages and controls the network, defines how data 
  packets should be transmitted.
  - **Data Plane:** Physically moves data based on control plane decisions.
- **Protocols Used**:
  - **Control Plane:** Utilizes protocols like OSPF, BGP, and others.
  - **Data Plane:** Uses protocols like Internet Protocol (IP) and Ethernet.

### Pre-Router Control Plane
Pre-Router control refers to an approach where each router within 
a network has its own dedicated control plane.

- Inside every router there is a local forwarding table. The router operates
by matching bits with the datagram header with a table entry in the forwarding
table that specifies the appropriate output link to which datagram it should
be forwarded.

Now the main question is how this networking table is computed?
- In the earlier days, forwarding table used to be hand computed.
- Nowadays, forwarding tables are computed rather than hand configured.
- Now how they are computed is the difference between the routing approach
and software defined approach.
- In the traditional routing approach mentioned here, a distributed 
routing algorithm is present in every router. Means, a piece of the algorithm
is present in every router.

### Software-Defined Networking(SDN) control plane
In SDN, there is a remote control software that computes 
and distributes the forwarding table to use by each and every router under
its control.

- The router still performs the local forwarding, but the forwarding table
is received from the SDN. Unlike a Pre-Router Control Plane where it computes
by itself.

### Router Architecture
- The Router has some inputs and outputs. Here is where Physical layer and link layers
are implemented. Ethernet mostly implements Link layers. Key parts of the
network layers are also implemented in the inputs and outputs port.
- The Number of ports in a router can vary a lot. From 5 to 10 for home router to
Hundreds of ports on an industrial level router.
- Packets are moved from input port to output port using switching technique.
- This is actually the heart of the router. This is also a network within
a network.
- Router also has a routing processor. Which controls plane function, controls
switch fabric, and also control the forwarding table.
- As you can see from the figure, router components also can be devided into
two parts. 
  - The Control Plane
  - The Data Plane

<img src="images/Router-structure.png" style="width:80%;height:80%;"> <br>

### Input port functions
- **The line termination function:** This part is responsible for receiving
bit level transmission for receiving over the physical medium which is copper,
fiber or wireless. 
- **Link Layer:** Then there are link level functions, where bits are 
assembled in link layer frame. Like Ethernet.
- **Network layer function:** Finally, there are network layer functions here.
Packet ques may form here.
  - The most important part of the input port is to look up and forwarding
  functions. Determining the output port. To which port it will be forwarded.

<img src="images/Router-structure.png" style="width:80%;height:80%;"> <br>

This look up and forwarding is match plus action behavior. There are two
types of forwarding:
1. **Destination-based forwarding:** forward only based on destination
IP address (traditional).
2. **Generalized forwarding:** Forward based on any set of header field
values.

<img src="images/destination_based_forwarding.png" style="width:80%;height:80%;"> <br>

As from the table, we can see certain ranges has been booked. But what 
happens if the ranges intercept?

### Longest prefix matching
- Longest Prefix Matching (also known as Maximum Prefix Length Match) is an
algorithm used by routers in Internet Protocol (IP) networking to select 
an entry from a routing table.
#### How does Longest Prefix Matching work
- When a router receives an IP packet, it compares the destination IP 
address bit-by-bit with the prefixes in its routing table.
- The router selects the prefix with the most matching bits as the one to 
use for forwarding.
- Essentially, it prefers the longest prefix (i.e., the most specific prefix) 
that matches the destination IP address.

#### Examples of Longest Prefix Matching
1. **Example 1**:
  - Imagine the router receives an IP packet with the destination address 
**192.168.2.82**.
  - In binary, the IP address looks like this:
    - Destination IP address (binary): **11000000.10101000.00000010
    .01010010**
  - The router has the following prefixes in its routing table:
    - **192.168.2.80/29** (binary: **11000000.10101000.00000010.01010000**)
    - **192.168.2.64/27** (binary: **11000000.10101000.00000010.01000000**)
    - **192.168.2.0/24** (binary: **11000000.10101000.00000010.00000000**)
    - All of the prefixes above match our destination IP address. However, 
    if we compare the bits, we find that **192.168.2.80/29** matches the most 
    bits with IP address **192.168.2.82**. Therefore, this is our **“longest 
    prefix”** for this destination.

2. **Example 2**:
  - Now, consider an IP packet with the destination address **10.4.1.62**.
  - In binary, the IP address looks like this:
    - Destination IP address (binary): **00001010.00000100.00000001.00111110**
  - The router's routing table includes these prefixes:
    - **10.4.1.32/27** (binary: **00001010.00000100.00000001.00100000**)
    - **10.4.1.0/24** (binary: **00001010.00000100.00000001.00000000**)
    - **10.0.0.0/8** (binary: **00001010.00000000.00000000.00000000**)
  - Among these, **10.4.1.32/27** is the closest match to the destination 
IP address.

- Longest Prefix Matching often performed in `Ternary content addressable
memories (TCAM)`
  - Matching with prefix and retrieving the proper addressing table is done
  within one clock cycle.

### Switching Fabric
Switching Fabric is on the very heart of the router. Its job is to switch
packets from the input side to the output side of the switching fabric.
In other words, its job is to move packets from input port to output port
that has been determined by the longest prefix match.

- **Switching Fabric:** One of the most important fabric of switching is 
the switching rate. It is the rate at which packets can be transferred
from inputs to outputs.

**Three major types of switching fabrics:**
1. Memory:
   - The first routers were traditionally computers.
   - So the Switching between the input and output ports was direct
   control of CPU. It was a sort of routing processor.
   - The input port and output ports were operated as traditional IO
   devices in a traditional operating system.
   - The input port with an arriving packet will signal the CPU via
   interrupt. So that, the packet could be buffered from input to
   the processor memory.
   - The CPU will look for the appropiate port in the forwarding table
   - And write that content into the output buffer.
2. Bus:
   - Rather than taking a packet from input port to memory and from memory
   to output port, switching via BUS switches that intermediary and allows
   an input packet to directly write the output port buffer.
   - In this case the switching speed is limited to BUS bandwidth.
3. Interconnection network: This part is the most used switching fabric.
   - In the interconnection network, there are crossbar switches which 
   connect us from N input to N output.
   - Multi-stage switching networks are used
   - These multi-stage switch networks are made up by interconnecting smaller
   size switch elements both serially with multiple linear stages and in
   parallel across a given stage.
   - Fragment datagram into fixed length cells on entry.
   - switch cells through the fabric, reassemble datagram at exit.
   - As we have learned, parallelism can be exploited to build high
   performant switches such as a single router can habe 100's of Tbps
   bandwidth.

### Input port queuing
- When multiple input ports send packets to the same output port 
simultaneously, we need to deal with the fact that the input arrival rate 
is higher than the output departure rate. 
- This phenomenon is also named as, Head of the line blocking. HOL occurs
when packets from different input ports want to go the same output port.

<img src="images/HOL-blocking.png" style="width:80%;height:80%;"> <br>

### Output port queuing
- In output, bits can arrive in the N*R rate to the switch fabric, but the
bits can only be drained or transmitted out at the rate of R.
- When the arrival rate exceeds the departure rate, the buffer will fail, since
the buffers are finite; there may not be enough buffer space. Hence,
there can be packet loss.
- It's right at the output port where packet loss occurs.
  - Since there is finite buffer space, we need to drop some packets.
  If we are dropping some packets, we need to figure out a `DROP POLICY`
  on which packets to drop.
  - As there will be many packets in the buffer, we also need to figure
  out to which packet should be prioritized more than the others. We can
  call it, `Schedule Decipline`.

### Packet Scheduling
#### First come first serve (FCFS)
- In FCFS, packets are transmitted in the order they arrive to the output
port. 
- It is also known as First in first out.

#### Priority
- Priority scheduling works as the name suggests.
- In the priority scheduling, packets arriving in the output queue are
classified in priority classes.
- The priority queue discipline will transmit a packet from the highest
priority class that has a non-empty queue. That is packet waiting for
transmission in the same priority class are typically done, in first come
first serve manner.
- Now one might ask how the priority classes are decided. It mostly
depends on the ISP.

#### Round Robin Scheduling
- Round Robin works almost as Priority scheduling
- When a packet arrives at the output buffer, it also classifies the packets
into different priority classes.
- In the round bin, servers cyclically scan different classes. For example,
RR will transmit a packet from priority class A, then B and then C. Say
priority class B has no packets queuing, RR will go scanning from A to C.

#### Weighted Fair Queuing
- WFQ is a generalized version of Round Robin.
- WFQ schedule serving classes in Round Robin manner. Say there are three
classes. First serving class 1, then serving class 2 and then serving class 3.
Repeating the service manner.
- Say an output has R throughput. In WFQ, a specific priority class will
receive at least `W(i)*R` throughput. 
- WFQ allows some type of bandwidth to be made on a per-class basis.

### Network Neutrality
Network neutrality, often referred to as net neutrality, is the principle 
that Internet service providers (ISPs) must treat all Internet 
communications equally. Some `clear and bright line rules are` -
- **No Blocking:** The ISP cannot block lawful content, applications and
services or non-harmful devices, subject to reasonable network management.
- **No Throttling:** Shall not impair or degrade lawful Internet traffic on
the basis of Internet Content, application or service.
- **No Paid Prioritization:** Shall not engage in paid prioritization.

### IPV4 Datagram
<img src="images/IP-Datagram.png" style="width:80%;height:80%;"> <br>

1. **Version Number**:
    - The first **4 bits** specify the IP protocol version of the datagram. 
   For IPv4, this value is set to **4**.
    - Different IP versions (such as IPv6) have distinct datagram formats.
    - The version number helps routers interpret the rest of the IP datagram.

2. **Header Length**:
    - These next **4 bits** determine where the actual data begins within 
   the IP datagram.
    - Since IPv4 datagrams can include variable-length options, this field 
   specifies the start of the data.
    - Most typical IPv4 datagrams have a **20-byte header**.

3. **Type of Service (TOS)**:
    - The TOS bits allow differentiation between various types of IP 
   datagrams.
    - For example, real-time datagrams (used by IP telephony) can be 
   distinguished from non-real-time traffic (like FTP).
    - The specific level of service is determined by the router's 
   administrator.

4. **Datagram Length**:
    - This field represents the total length of the IP datagram (including 
   both header and data), measured in **bytes**.
    - It is a **16-bit** value, allowing a theoretical maximum size of 
   **65,535 bytes**.
    - However, actual datagrams are rarely larger than **1,500 bytes**.

5. **Identifier, Flags, and Fragmentation Offset**:
    - These three fields relate to **IP fragmentation**:
        - **Identifier**: Helps reassemble fragmented datagrams.
        - **Flags**: Indicate whether fragmentation is needed or if more 
      fragments follow.
        - **Fragmentation Offset**: Specifies the position of each 
      fragment within the original datagram.

6. **Time-to-Live (TTL)**:
    - The TTL field ensures that datagrams do not circulate indefinitely 
   due to routing loops.
    - Each router decrements the TTL by one; if it reaches **0**, the 
   datagram is dropped.

7. **Protocol**:
    - Used only at the final destination.
    - Indicates the specific transport-layer protocol to which the data 
   portion of the IP datagram should be passed.
    - For example, a value of **6** indicates TCP, while **17** indicates 
   UDP.

### Classful IP addressing
**Classful IP addressing** is a method of **IP address allocation** in 
which IP addresses are divided into predefined classes. These classes are 
designated by the first few bits of the IP address, determining both the 
network and host portions of the address. Let's explore the details of 
class addressing:

1. **Class Divisions**:
    - Classful addressing divides the **IPv4 address space** (ranging from **0.0.0.0** 
   to **255.255.255.255**) into five classes: **A, B, C, D, and E**.
    - However, only classes **A, B, and C** are used for network hosts:
        - **Class A**: Suitable for very large networks. The network ID is 
      **8 bits** long, and the host ID is **24 bits** long. The first 
      octet's higher-order bit is always set to **0**. The default subnet 
      mask for Class A is **255.x.x.x**.
        - **Class B**: Assigned to medium-sized to large-sized networks. 
      The network ID is **16 bits** long, and the host ID is also **16 
      bits** long. The first octet's higher-order bits are always set to 
      **10**. The default subnet mask for Class B is **255.255.x.x**.
        - **Class C**: Suitable for small networks. The network ID is 
      **24 bits** long, and the host ID is **8 bits** long. The first 
      octet's higher-order bits are always set to **110**. The default 
      subnet mask for Class C is **255.255.255.x**.

2. **Class D and E**:
    - **Class D** (IP address range: **224.0.0.0 - 239.255.255.255**) is 
   reserved for **multicast** purposes.
    - **Class E** (IP address range: **240.0.0.0 - 255.255.255.255**) is 
   reserved for **future use**.

### Subnet
A **subnet**, or **subnetwork**, is a logical subdivision of an IP network. 
Here's how it works:

- **Purpose**: Subnetting makes networks more efficient by dividing a 
larger network into smaller segments.
- **Addressing Within Subnets**:
    - Computers within the same subnet share an identical group of the 
  most significant bits of their IP addresses.
    - This logical division results in two fields:
        - **Network Number (Routing Prefix)**: The part that indicates the
      network.
        - **Host Identifier**: The part that specifies a specific device 
      within that network.
    - For example:
        - The prefix **198.51.100.0/24** has a subnet mask of 
      **255.255.255.0**.
        - Addresses from **198.51.100.0** to **198.51.100.255** belong 
      to this subnet.

- **Routing and Efficiency**:
    - Traffic between subnets is routed through routers.
    - Subnetting ensures that packets take a direct route to their 
  destination without unnecessary detours.

### CIDR Address
- **CIDR** is a method of representing IP addresses and their associated 
subnet masks in a more flexible and concise way.
- It allows for finer control over address allocation and efficient 
utilization of IP address space.
- In CIDR notation, an IP address is followed by a **slash (/)** and a 
number (e.g., **192.168.0.0/24**).
- The number after the slash represents the **prefix length** (also known 
as the **subnet mask length**), indicating how many bits are used for the 
network portion of the address.

### How CIDR Is Used for Subnetting:
1. **Variable-Length Subnet Masks**:
    - CIDR allows for **variable-length subnet masks** (VLSMs) instead of 
    the fixed-length masks used in classful addressing.
    - With VLSMs, you can create subnets of different sizes within the 
   same IP address range.
    - For example, you can divide a Class C address into smaller subnets 
   with varying numbers of hosts.

2. **Efficient Address Allocation**:
    - CIDR enables efficient allocation of IP addresses by allowing 
   administrators to allocate only the necessary number of addresses for 
    each subnet.
    - Instead of allocating fixed blocks (e.g., Class A, B, or C), CIDR 
   allows custom-sized subnets based on actual requirements.

3. **Aggregation and Routing**:
    - CIDR facilitates **route aggregation** by summarizing multiple 
   smaller subnets into a single route.
    - Routers can advertise summarized routes, reducing the size of 
   routing tables and improving routing efficiency.

4. **Example**:
    - Suppose you have a block of IP addresses **192.168.0.0/24** 
   (which means a subnet mask of **255.255.255.0**).
    - You can further divide this into smaller subnets:
        - **192.168.0.0/25** (subnet mask: **255.255.255.128**): 128 
      addresses
        - **192.168.0.128/26** (subnet mask: **255.255.255.192**): 64 
      addresses
        - And so on...

5. **CIDR Cheat Sheet**:
    - Here's a quick reference for CIDR notation and their corresponding 
   subnet masks:
        - /24: 255.255.255.0
        - /25: 255.255.255.128
        - /26: 255.255.255.192
        - /27: 255.255.255.224
        - And so forth...
