# The Network Security
### IP Security (IPSec)
IP Security, commonly known as **IPSec**, is a suite of protocols used to 
secure Internet communications by authenticating and encrypting each IP packet 
of a communication session. IPSec provides a robust security layer for the 
network traffic across IP networks. Here are some key points about IPSec:

- **Data Authentication**: IPSec ensures that the data originates from a known 
sender, providing authenticity.
- **Data Integrity**: It guarantees that the data has not been altered during 
transmission.
- **Confidentiality**: Through encryption, IPSec ensures that the data cannot 
be read by unauthorized parties.
- **Anti-Replay**: It protects against unauthorized transmission of packets, 
ensuring that each data packet is unique and not duplicated.

IPSec operates in two modes:
- **Transport Mode**: Encrypts only the payload of the IP packet, leaving the 
header untouched.
- **Tunnel Mode**: Encrypts both the payload and the header, which is used for 
VPNs.

### What is Asymmetric Encryption? How is it used in IPSec?
Asymmetric encryption, also known as public-key cryptography, is a method of 
encrypting data that uses two different keys: a **public key**, which can be shared
with everyone, and a **private key**, which is kept secret. The public key is used
to encrypt data, and the corresponding private key is used to decrypt it. This 
ensures that only the intended recipient, who possesses the private key, can 
access the original data.

In the context of IPSec, asymmetric encryption is primarily used during the 
**Internet Key Exchange (IKE)** phase, which is the initial setup of the IPSec 
connection. Here's how it works in IPSec:

1. **Establishing a Secure Channel**: When two devices initiate communication, they use 
asymmetric encryption to securely exchange the symmetric keys that will be used for the 
session.
2. **Authentication**: Asymmetric encryption helps in authenticating the devices to each 
other, confirming that the data is being sent and received by the correct parties.
3. **Key Exchange**: The public key encrypts the symmetric keys, and only the holder of the 
private key can decrypt them, ensuring the security of the key exchange process.

Once the secure channel is established and the symmetric keys are exchanged, IPSec typically 
switches to **symmetric encryption** for the actual data transfer because it's faster and 
more efficient for continuous data encryption and decryption.

This combination of asymmetric and symmetric encryption allows IPSec to provide a balance 
between strong security during the key exchange and efficient data transfer during the 
communication session.

### How does IPsec work? Key exchange, packet headers and trailers, authentication, encryption, transmission, and decryption.
IPsec (Internet Protocol Security) is a suite of protocols that ensures secure communications 
over IP networks through the use of cryptographic security services. Here's a high-level 
overview of how IPsec works, covering key exchange, packet headers and trailers, 
authentication, encryption, transmission, and decryption:

1. **Key Exchange**:
    - IPsec uses the Internet Key Exchange (IKE) protocol to establish a secure and 
   authenticated channel for negotiating the keys used for encryption and decryption.
    - IKE operates in two phases:
        - **Phase 1**: Establishes a secure and authenticated channel (the IKE 
      Security Association).
        - **Phase 2**: Negotiates the IPsec Security Association (SA) parameters 
      and sets up the actual IPsec tunnel for data transfer.

2. **Packet Headers and Trailers**:
    - In **Tunnel Mode**, IPsec encapsulates the entire IP packet, adding an 
   IPsec header and trailer around the original packet. The IPsec header 
   typically includes the new IP header, and the trailer contains the original 
   IP header and payload.
    - In **Transport Mode**, only the payload of the IP packet is encapsulated, 
   leaving the original IP header intact.

3. **Authentication**:
    - IPsec provides authentication at the packet level, ensuring that the packets
   originate from a trusted source.
    - Authentication can be provided by the Authentication Header (AH) protocol 
   or as part of the Encapsulating Security Payload (ESP) protocol[^10^].

4. **Encryption**:
    - The ESP protocol is responsible for encrypting the payload of the IP packet 
   to ensure confidentiality[^10^].
    - Encryption is performed using symmetric cryptography, where both the sender 
   and receiver share a common key.

5. **Transmission**:
    - Once the IP packet is prepared with the necessary headers, trailers, and 
   encryption, it is transmitted over the network to the destination.
    - IPsec can use various transport protocols, but it often uses UDP to 
   facilitate NAT (Network Address Translation) traversal.

6. **Decryption**:
    - Upon receiving the encrypted IP packet, the recipient uses the shared 
   symmetric key to decrypt the payload.
    - If authentication is used, the recipient also verifies the packet's 
   authenticity before processing the payload.

### What protocols are used in IPSec?
#### Authentication Header (AH)
This protocol provides authentication of the origin of the data and guarantees 
the integrity of the transmitted data. It does not provide encryption but ensures 
that the packet has not been tampered with in transit.

### Encapsulating Security Payload (ESP)
ESP provides confidentiality by encrypting the payload of the IP packet itself. 
It also provides authentication of the origin and integrity of the data, similar to 
AH, but with the added benefit of encryption.

### Security Association (SA)
This is not a protocol per se, but a concept within IPsec. An SA is a relationship 
between two or more entities that describes how they will use security services to 
communicate securely. It includes all the information required for the execution of 
various IPsec protocols, such as the encryption and authentication algorithms being 
used and the keys to use them.

### Internet Protocol (IP)
While not exclusive to IPsec, IP is the network layer protocol through which 
data packets are transmitted from one host to another.

### IPSec Tunnel Mode Vs IPSec Transport Mode
- **IPsec Tunnel Mode**:
   - Encapsulates the **entire IP packet**, including both header and 
  payload, within a new IP packet with a new IP header.
   - Typically used for **network-to-network** communications, such as 
  connecting two different networks over the internet using a VPN.
   - Provides **end-to-end security** by encrypting the entire IP packet, 
  which is useful for protecting traffic between different networks.

- **IPsec Transport Mode**:
   - Only the **payload** of the IP packet is encrypted, while the original 
  IP header remains intact.
   - Generally used for **end-to-end communication** between hosts, such as 
  secure data transfer between two computers.
   - Since a new IP header isn't created, the process is less complex than 
  tunnel mode.

### Which Port IPSec use?
IPsec primarily uses the following ports and protocols for its operations:

- **UDP port 500**: This port is used by the Internet Key Exchange (IKE) 
protocol on the management plane to establish Security Associations (SAs) 
and manage encryption keys.
- **UDP port 4500**: This port is utilized for IPsec NAT-Traversal (NAT-T), 
which allows IPsec traffic to pass through NAT devices.
- **IP protocol 50**: Known as Encapsulating Security Payload (ESP), it is 
used for securing the data flow in IPsec.
- **IP protocol 51**: This is the Authentication Header (AH) protocol, 
which provides connectionless integrity and data origin authentication for 
IP datagrams.

### How does IPSec impact MSU and MTU?
IPsec affects the Maximum Segment Size (MSS) and the Maximum Transmission Unit (MTU) due 
to the additional headers it introduces for security purposes:

- **Maximum Segment Size (MSS)**: IPsec adds overhead to the original packet size, which 
includes the IPsec headers and trailers. Therefore, when using IPsec, the MSS must be 
adjusted to account for this overhead to avoid fragmentation.
  - MSS = MTU - (TCP header + IP header + IPSec overhead)

- **Maximum Transmission Unit (MTU)**: When IPsec is used, the MTU must be reduced
to accommodate the IPsec overhead, which includes the ESP(Encapsulating Security Payload) 
header, trailer, and any additional padding. This reduction ensures that packets do not 
exceed the MTU of the network path, which could lead to fragmentation.

### Security Features
#### Authentication
This is the process of verifying the identity of a user, device, or entity. It ensures 
that the entity is who it claims to be. Common methods include passwords, digital 
certificates, and biometric verification.

#### Authorization
Once authenticated, authorization determines what resources a user or entity has access 
to and what actions they are permitted to perform. It’s about granting or denying rights 
and privileges.

#### Privacy
This refers to the right of individuals or organizations to keep their information 
confidential and to determine how their data is collected, used, and distributed.

#### Confidentiality
Similar to privacy, confidentiality is about protecting information from being accessed 
by unauthorized parties. It ensures that sensitive information is only disclosed to those 
who have the right to access it.

#### Integrity
This security feature ensures that data is not altered or tampered with during transmission 
or storage. It guarantees that information remains accurate and consistent throughout its 
lifecycle.