A **computer network** is a network of inter-connected devices which enable **processes running on different devices** to communicate
- Processes communicate by sending **messages**
- **Intermediate nodes** help forward messages
### Components of a network
- **Network edge** - Consists of **end hosts** which run **network applications**
	- e.g. Web, Email, Video Streaming, Online Games
	- Applications involve two **communicating** processes running on two **different** end hosts
- **Network core** - Consists of **packet switches** which help forward data packets
	- e.g. switch and router
- **Communication links** - Carry data **between** networks devices as EM waves
	- e.g. fiber, copper, radio, satellite links
	- Each link has a finite **bandwidth** or **speed** which limits the rate at which data can be sent

![[Pasted image 20231017140817.png]]
#### Functions of an End-Host
**Must**:
- Run application processes which generate messages
- Breaks down application messages into smaller chunks called **packets**
- Adds additional information (e.g. *IP address*, *port no*) as **packet headers** so that the packets can be carried by the internet to their destinations
	- **IP address** - Uniquely identifies an **end host** in the network
	- **Port no** - Uniquely identifies a **process** running within an end host
- Send bits over a **physical medium**
**Optional**:
- If needed provide **reliable** and **orderly** delivery of packets
- **Controls** the **rate** of transmission of packets
#### Functions of Network Core
**Routing** - Run **routing algorithms** to **construct** routing tables
**Forwarding** - Once a packet arrives, the device checks the **address** of the packet (stored in its *header*). It is forwarded to the appropriate **output link** according to the **routing table**

![[Pasted image 20231017083712.png]]

IP addresses are grouped together to certain output links, going from very general to localised links.
#### Four Sources of Packet Delay
##### $d_{trans}$ - **Transmission delay**:
- $L$ - Packet **length** (bits)
- $R$ - Link **bandwidth**
- $d_{trans}$ = $\frac{L}{R}$

**Store and forward** - **Entire** packet must **arrive** at router **before** it can be **transmitted** on next link
- Takes $\frac{L}{R}$ seconds to **transmit** $L$-bit packet into link at $R$ bps
- **End-to-end** (transmission) delay = $\frac{2L}{R}$ sec

![[Pasted image 20231017142610.png]]

If $L$ is **large**, it would take a very **long time** to send messages since end-to-end delay is **increased**
- Better to **break down** messages into smaller chunks to **reduce** delay

##### $d_{queue}$ - **Queueing delay**:
- Time **waiting** at output link for transmission
- Depends on **congestion** level of **router**

If **arrival rate** to link exceeds **transmission rate** of link for a period of time:
- Packets will queue, **wait** to be transmitted on link
- Packets can be **dropped** (lost) if memory (buffer) **fills up**

![[Pasted image 20231017084156.png]]

##### $d_{proc}$ - **Nodal processing**:
- Check bit **errors**
- Determine **output** link
- Typically < millisecond
##### $d_{prop}$ - **Propagation delay**:
- $d$ - **Length** of physical link
- $s$ - Propagation **speed** in medium (~2x10$^{8}$ m/s)
- $d_{prop}=\frac{d}{s}$
##### Diagram of all delay types
![[Pasted image 20231017085917.png]]
#### Throughput
Throughput is specific to a **flow** or **communicating pair**
- It is the rate at which bits are **transferred** from source to destination in a given **time window**
	- **Instantaneous**: Rate at **given** point in time
	- **Average**: Rate over a **longer** period of time

![[Pasted image 20231017143721.png]]

![[Pasted image 20231017143920.png]]

- If $R_{S}<R_{C}$, average end-end throughput is $R_{C}$
- If $R_{S}>R_{C}$, average end-end throughput is $R_{S}$

**Bottleneck link**
Link on end-end path that has **minimum** speed **constrains** the throughput
### Protocols
A protocol defines **rules** communication
- **Sequence** and **format** of messages
- **Actions taken** on message **transmission** and **receipt**
Network protocols can be implemented **either** as **software** or as **hardware**
### Packet vs Circuit switching
#### Packet Switching
- The internet uses **packet switching** technology
- Different **flows** (*source-destination pairs*) **share** resources (*link*) along their **routes**
- Internet traffic is **bursty** in nature - if one flow is **not** using a link then the **other** flows **can** use it
- Flows can **change** routes if link **fails** or becomes **congested**

![[Pasted image 20231017144948.png]]

Resources are **not** pre-allocated to a communicating pair of devices
- **Cons**:
	- **No** rate guarantee
	- **Losses** possible
- **Pros**:
	- Better **utilization** of **resources**
#### Circuit Switching
- **Before** the internet, circuit switching was used in telephone networks
- A **circuit** consists of all communication links along a path from a source to destination
- In circuit switching, a **circuit** is **reserved** for each flow for the **entire** call duration
- **Guaranteed** **rate** of communication
- Flows do **not share** resources
- If one flow is **not** using its assigned circuit during the call, it **cannot** be used by **another flow**. Not ideal for bursty internet traffic
- If a link **fails**, call must **end**

![[Pasted image 20231017145527.png]]

Resources are **reserved** for a communicating pair for the **entire** duration of communication. Called a **circuit**.
- **Cons**:
	- Poor **utilization** of **resources** for bursty traffic
- **Pros**:
	- **Guaranteed** rate - you know the throughput exactly
	- **No** losses
### Design Philosophy: Layering
Network devices perform **complex** functions - it is better to **divide** the functions into "*layers*"
- Each layer performs a **subset** of functions
	- Layer $N$ **uses** the services of layer $(N-1)$ and **provides** services to layer $(N+1)$
- Layering makes it easier to **add** services to a layer or **change** its implementation without **affecting** other layers
- The internet protocol stack has 5 layers containing all possible functions
	- All layers may not be present in a network device
##### 5. Application Layer
**Generates** data to be **communicated** over the internet
*HTTP, SMTP, DNS*
- If you're developing an application for, e.g. Web or Email, you need to **follow** the HTTP/SMTP rules 
	- **Guarantees** that all rules are **followed** by browsers and developers so that it works between devices
##### 4. Transport Layer
**Packetize** the data, add **port no**., add **sequencing**  and **error correcting** info
*TCP (guarantees every packet reaches destination), UDP (does not guarantee packets e.g. online games - faster)*
- Takes the messages and breaks them down into **smaller packets**, and adds "*Transport Layer*" header with extra info above
	- **Port number** - Identifies **source** and **destination** process
	- **Sequencing** - Helps identify **lost** packets and **order** of packets
##### 3. Network Layer
Add source and destination **IP addresses**, run **routing algorithm**
*IP, Routing protocols*
- **Routing algorithms** - Determine **paths** that packets take to arrive at destination
- **IP protocol** - Specifies how IPs are assigned and used
##### 2. Link Layer
Add source and destination **MAC addresses**, pass **frames** onto NIC drivers
*Ethernet, WiFi*
- **MAC addresses** - Identify the **hardware** interface cards
	- Between every hop, the source and destination MAC addresses **change** since the packet travels between two **different** interfaces each time
##### 1. Physical Layer
Send **individual bits** through the physical communication link
Separate protocol for each **physical medium**: *co-axial cable, WiFi, etc.*
#### Encapsulation

![[Pasted image 20231017174959.png]]

While travelling through each layer, it **strips** off the **necessary** information - its **designated** **header** - network layer would, for example, not see the link information - only the network head information **specific** to it
- Every time the message goes **up** a layer the header gets **removed**, when it goes **down** a layer the header gets **added**
- Application layer would therefore **just** see the **message**, **without** any headers

- Switch (aka *Link Layer switch*) only runs the **bottom 2 layers** - Link and Physical
- Router runs bottom **3 layers** - Network, Link and Physical