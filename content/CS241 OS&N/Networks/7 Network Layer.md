**Main functions** of the network layer:
- **Move** packets from the **source node** to **destination node** through **intermediate nodes** (routers)
- Network layer runs in **end hosts** and **routers**
- One of the main protocols running in the network layer is *IP*
	- IP at **source**
		- **Adds IP header**, containing **src** and **dest** IP addresses to transport layer segments and sends them to link layer below
	- IP at **routers**
		- **Checks** the destination IP address of incoming packets to **decide** the **next** hop router
	- IP at **destination**
		- **Receives** IP datagram, **strips** IP header and **delivers** to transport layer 
	- There are **other functions** of the IP protocol
		- e.g., fragmenting oversized packets and reassembling them at the destination
- Network layer in the routers run **routing protocols** (e.g., RIP, OSPF) to compute routes
## Routers
**Two** key **functions** of routers:
- **Forwarding**
	- **Move** packets **from** router's **input** **to** appropriate router **output**
- **Routing**
	- Construct **routing table**
		- Routing **protocols**
	- ![[Pasted image 20231228171640.png]]
### Routing table
- $2^{32}$ (≈4 billion) **possible** IP addresses 
- **Impractical** to keep a **separate entry** for each IP 
- **Increases** the **size** of **routing table**, look up time 
- **Many** IPs map to the **same outgoing link** 
- Better if each entry corresponds to a **group** of IPs instead of an individual IP

![[Pasted image 20231228181556.png]]

Each routing table looks something like this:

![[Pasted image 20231228181608.png]]

Where **each interface** is **associated** with **multiple** IP addresses
However, what happens if the ranges have overlap with their IP addresses?

**Longest prefix matching** is used:
- When looking for a forwarding table entry for given destination address, use **longest** address prefix that matches destination address

![[Pasted image 20231229175804.png]]

- In first example, since the number of matching digits is largest (**in common**) with interface 0, this one is chosen
- Same for second example
### IP addressing and subnets
- IPv4 addresses are 32 bit addresses which **uniquely** identify **network interfaces**
- **Represented** in dotted decimal
- ![[Pasted image 20231229182935.png]]
- IP addresses belonging to the **same** **subnet** has the same prefix: **subnet mask**
- Interfaces belonging to the **same subnet** are connected by a **link layer switch** and can **communicate directly** with each other
	- A link layer switch uses MAC addresses to **forward** link layer frame from src to dest, both belonging to the same subnet

![[Pasted image 20231230175323.png]]

- With each IP address its subnet mask is also specified using the **number of bits** used in the **prefix**
- **CIDR (Classless Inter-Domain Routing)** notation: 
	- a.b.c.d/x
- A sender first checks if the destination IP has the **same prefix** as its subnet mask. If so, then 
	- Obtain the MAC address of the **dest** (through ARP) 
	- Create a link layer **frame** 
	- **Forward** it to the link layer switch
#### Default gateway
- If src and dest belong to **different subnets**, then src forwards the packet to its **default gateway** 
	- A gateway router **connects** one subnet to other subnets 
- If A wants to **communicate** with B, then A will **forward** its packets to R, the **default gateway** 
- After receiving a packet from A, R will **lookup** its routing table to forward it to the right **outgoing interface** 
- Once the packet reaches this interface, it can be **forwarded** to B through the **switch** in subnet 2
### IP addresses: how to get one?
How does a **node** get an IP address? **Two options**: 
- **Network admins** can **manually** configure IP address of **each host** in a network 
	- UNIX based systems **store** network configuration in a **system file**
		- e.g., /etc/rc.config 
- **DHCP** (**Dynamic Host Configuration Protocol**): 
	- Application layer protocol that **dynamically assigns** IP address from a **server** (usually the gateway router) **to clients** (nodes trying to connect to the subnet) 
- In either method, **subnet mask** and **default gateway** must be **specified**.

We have seen so far how a **host** gets IP address in a network
- But how does the **network** get subnet part of IP addr?

- It gets allocated portion of its **provider** ISP’s **address space** 
- Global authority ICANN is ultimately responsible for allocating IP addresses to ISPs

![[Pasted image 20231230180244.png]]
### NAT: Network address translation
- IPv4 addresses are in short supply
	- (ICANN has given out its last block of IPv4 addresses in 2011) 
- It is **not possible** for ISPs to provide a unique IP to each device connected to a subnet 
- Instead a globally **unique public IP address** is provided to only the gateway routers 
- Then, what about other devices? 
	- We assign private IP addresses which are not unique globally but are unique in a **subnet**

**Public** and **Private** IP addresses:
- Devices in **home** or **private** networks need **not** be **visible** to the public internet, they can use **private** IP addresses to communicate with each other 
- The ICANN **reserves** the following IP address blocks for use as **private** IP addresses: 
	- 10.0.0.0 to 10.255.255.255 
	- 172.16.0.0 to 172.31.255.255 
	- 192.168.0.0 - 192.168.255.255 
- Packets having private IP addresses **cannot** be carried by the **public internet**

![[Pasted image 20231230191232.png]]

- NAT converts private **source** IP address of **private hosts** to the **public** IP address of the **router** facing the internet 

But how is the router going to distinguish between replies meant for different private hosts?

![[Pasted image 20231230191638.png]]

NAT is controversial: 
- Port numbers should be used to identify **processes** **not hosts** 
- Address shortage should instead be **solved** by **IPv6** (128 bit addresses)
## Routing 
- At each router, a **routing protocol** (e.g., RIP, OSPF) **constructs** the **routing table**
- Each routing protocol implements a **routing algorithm** 
- We shall study routing algorithms instead of routing protocols

![[Pasted image 20231230193939.png]]
#### Graph abstraction:

![[Pasted image 20231230194358.png]]
### Routing algorithms
**Two types** of routing algorithms:
- **Global**
	- Requires the knowledge of the **complete topology** at each router including costs
	- Link state algorithms
- **Local**
	- Requires the knowledge of only **local neighbourhood** at each router
#### Link-state routing algorithm:
- Computes **least cost paths** from one node (‘**source**’) to **all other nodes** 
- Implemented in **Open Shortest Path First (OSPF)** protocol 
- Each node requires the **entire topology** including the **cost of each link** 
- Obtained through **broadcasting** of **link costs** (or ‘link states’) 
	- Each node broadcasts the cost of each link connected to it to all other nodes in the network 
	- At the end of broadcast message exchanges, all the nodes have the **same global picture** of the network

Source node $u$

![[Pasted image 20231230195116.png]]

![[Pasted image 20231230195125.png]]
#### Distance Vector (DV) algorithm
- DV is implemented in the **Routing Information Protocol (RIP)** 
- Unlike Dijkstra’s algorithm, DV uses **local information** from **neighboring nodes** to compute **shortest paths**
- The algorithm is based on **Bellman-Ford equation**

![[Pasted image 20231230200009.png]]

- $D_x(y)$ = current **estimate** of **minimum distance** from $x$ to $y$ (**different** from **actual** minimum distance $d_x(y)$) 
- DV algorithm tries to converge **estimates** to their **actual values** 
- Each node $x$ maintains **distance vector** $D_x = [D_x(y): y \in N]$ (vector of **current estimates**)
- Node $x$ performs **update** $D_x(y)=min_v\{c(x,v)+D_v(y)\}$ 
- To perform the the update node $x$ **needs** 
	- Cost to **each neighbor** $v: c(x,v)$ 
	- Distance vector of **each neighbour** $v: D_v = [D_v(y): y \in N]$ (obtained through **message passing**) 
- Whenever any of these is updated, the node **recomputes** its **distance vector** and sends the **updated** distance vector to **all its neighbours**

![[Pasted image 20231230200719.png]]

![[Pasted image 20231230200730.png]]

![[Pasted image 20231230200737.png]]

