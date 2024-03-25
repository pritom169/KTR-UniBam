# Lecture-1
*Q1. What is Internet?*
- The Internet is an interconnected network of networks.

*Q2. What is protocol?*
- Sending and Receiving information between Router, Switches and Many other end
devices is controlled by protocol. Protocol governs everything that happens in a 
network.
- Protocols define the format, order of messages sent and received among network
entities, and actions taken on message transmission, receipt.

> IETF: **Internet Engineering Task Force** governs the Internet standards using
**Request for Comments**

###### Access network
Access network is the host, (the device or the end system) that connects to
the internet. <br>
**How to connect host systems to edge router?** <br>
*Answer:* Here are some ways:
- Residential access nets
- institutional access nets(school, company)
- mobile access network(Wifi/4G)
**What to look for**
- transmission rate (bits per second) of access network. AKA how fast is the network.
- shared or dedicated access among the users (AKA to what degree the network is shared)

##### Access Networks: cable-based access
In access network, a single cable headed connects multiple homes. The signal from and
to those houses are sent at different frequencies. Different frequencies do not interpret
with each other just like FM radio frequencies.

- *Frequency division multiplexing(FDM):* Different channels multiplied in a different frequency
band.
- *HFC: Hybrid fiber coax:* They are asymmetric, which means they provide data in a downstream
manner rather than upstream. Which means we are consumer of data rather than producer of
data. Typical cable transmission rates are 40MBPS - 1.2GBs downstream transmission rate,
30-100Mbps upstream transmission rate. Usually your modem will limit the upstream and the
downstream limit. Basically, you get what you pay for.

##### Access networks: digital subscriber line(DSL)
- Use existing telephone line to central office DSLAM
    - data over DSL phone line goes to the Internet
    - voice over DSL phone line goes to telephone net
- 24-52 Mbps dedicated downstream transmission rate
- 3.5-16 Mbps dedicated upstream transmission rate

#### Access networks
##### Home networks
Traditional home network which has a device. It is connected with router and modems.

##### Wireless access networks
1. Wireless local area network (WLANs)
- Typically, within or around building (100ft)
- Typically, standardized by IEEE under the protocols of 802.11 b/g/n.

##### Wide-area cellular access networks
- provided my mobile, cellular network operator (10's km)
- 10's Mbps
- 4G cellular networks (5G coming)

Other access networks are:
1. Enterprise networks
2. Data center networks

#### Host: Sends packets of data
host sending function:
- take an application message
- breaks into smaller chunks, known as packets, of length L bits
- transmits packet into access network at `transmission rate R`
    - link transmission rate, aka link `capacity, aka link bandwidth`
> packet transmission delay = time needed to transmit an L-bit packet into 
a link = L(bits)/R(bits/sec)

#### Links: Physical Media
- `bit:` We transfer bits from transmitter to receiver using a physical media.
- `physical link:` what lies between transmitter & receiver.
1. `Guided media`: Signals propagate in solid media: copper, fiber, coax. There are
couple of guided medias. Some examples of guided media are the following:
- `Coaxial cable:`
  - two concentric copper conductors
    - bidirectional
    - broadband, old fashioned internet used to run on this method.
        - multiple frequency channels on cable
        - 100's Mbps per channel
- `Twisted pair(TP)`: two insulated copper wires
    - Category 5: 100 Mbps, 1Gbps Ethernet
    - Category 6: 10 Gbps Ethernet
- `Fiber optic cable:`
  - glass fiber carrying light pulses, each pulse a bit
    - high-speed operation:
        - high-speed point-to-point transmission (10's - 100's Gbps)
    - low error rate:
        - repeaters spaced far apart
        - immune to electromagnetic noise
2. `Unguided Media:` signals propagate freely e.g., radio <br>
`Wireless Radio`:
- signal carried in various "bands" in electro-magnetic spectrum.
- no physical "wire"
- broadcast, "half-duplex" (sender to receiver)
- propagation environment effects:
    - reflection
    - obstruction by objects
    - Interference/noise
`Radio link types:`
  - `Wireless LAN(WiFi):` 10-100's Mbps; 10's of meters.
  - `Wide-area (e.g., 4G cellular):` 10's Mbps over ˜10 Km
  - `Bluetooth:` cable replacement
    - short distances, limited rates
  - `Terrestrial microwave:` point-to-point; 45 Mbps channels
  - `Satelite`:
    - upto 45 Mbps per channel
    - 270 msec end-end delay