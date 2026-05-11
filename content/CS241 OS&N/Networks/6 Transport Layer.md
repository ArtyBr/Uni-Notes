The transport layer provides **logical communication** between app **processes** running on different hosts
Transport protocols run in **end systems**
- **Send side** - Breaks app messages into **segments**, **adds header**, passes to **network layer**
- **Receive side** - **Reassembles** segments into messages, passes to **app layer**

![[Pasted image 20231114225521.png]]

Transport layer protocol available to apps
- Internet: **TCP** and **UDP**
### UDP: User Datagram Protocol
- **UDP** provides the **bare minimum** services:
	- **Packetizes** application layer data, **adds ID header**, and **sends** them to the **network layer**
	- If the packets reaches the destination, UDP **delivers** them to the **right process**

- **No effort** is made to **recover** lose packets or **reorder** out of order packets
- UDP is **connection-less**
	- **Each** UDP segment is treated **independently**
- No **congestion control**
	- Using UDP, senders can send at **any rate** they wish even when the network is **congested**

**Benefits**:
**Fast** and **less overhead**
- No **connection establishment**
	- (Which can add **delay**)
- **Smaller** header size than TCP
- No **congestion control**:
	- UDP sender can blast away as fast as desired

For **reliable** transfer over **UDP**:
- Add reliability at **application layer**
### TCP: Transmission Control Protocol
**In addition** to the services provided by UDP, TCP also provides:
- **Reliable** data transfer:
	- **Recovers losses**
	- **Re-orders** out of order packets
- **Flow control**:
	- **Matches** the **sending speed** of the sender to the **reading speed** of the receiver
- **Congestion control**:
	- Controls the **sending** rate according to perceived **network congestion**

TCP **makes sure** that lost, corrupted or out of order packets arrive to the application **properly**
- Processes see **reliable communication channel**
Hence, TCP enhances the **unreliable** network layer service to a **reliable** transport layer service
## Reliable transfer over an unreliable channel
**General requirements** for building reliable data transfer over an unreliable channel
- In an **unreliable channel** there can be:
	- **Bit errors**
		- Bits can get **flipped** due to electrical noise
	- **Loss packets**
		- Packets can get **lost** due to buffer overflow at the routers
	- **Reordering of packets**
		- Packets may arrive in an order **different** from the order in which they are sent

- **Mechanisms needed**:
	- **Checksums**:
		- To detect **bit errors**.
		- Include the **sum** of all 16-bit words in the header
	- **Acknowledgements (ACKs)**:
		- To indicate if a packet is correctly **received** at the receiver
	- **Timeout** mechanism:
		- Sender **times out** if ACK is **not received** within a timeout **interval**
	- **Retransmissions**:
		- To **retransmit** lost or corrupted packets
		- **Automatic Repeat Request** (ARQ)
	- **Sequence number**:
		- To correctly **order** packets
### Stop and Wait ARQ
- Sender **sends** a packet
- **Waits** until it receives an **ACK**
- If ACK arrives, sends the next packet, **else** times out and **retransmits** the same packet

![[Pasted image 20231115091014.png]]

Packet needs to be tagged with the sequence number to prevent **lost ACK** 
- Ack never gets received by sender so it sends duplicate packet 
- Receiver thinks this duplicate packet is the second packet

![[Pasted image 20231115091348.png]]

Another case if not tagged with sequence number:

![[Pasted image 20231115091800.png]]

### Performance of Stop and wait ARQ

![[Pasted image 20231115091829.png]]

**Maximal throughput**: 1Gbps via UDP
- However not all of the 1Gb would actually arrive at receiver
How to make Utilisation closer to 1?
- Make RTT smaller?
- Make L very large?
- To make utilisation large make the $RTT = L/R$ 
	- Have to send to another planet to even make it 0.5

To **improve**:
- Sender should be allowed to send **more packets** without waiting for the ack
- Sender could send $R\times RTT$ bits of **additional** data during the $RTT$ interval
- $R\times RTT$ is called the delay-bandwidth product of the communicating pair and indicates the **length** of the **pipeline**
	- The amount of bits you can transmit **before** you receive the first ACK

![[Pasted image 20231115092812.png]]

The receiving process typically has a **finite buffer** of $B$ bits
- The receiving process may not be reading from its buffer all the time
- Hence, to **avoid** buffer **overflow**, the sender should **not** send **more** than $B$ bits at a time.
- Length of pipeline = $L+R\times RTT$
- Buffer Size = $B$ bits
- Max no bits without waiting for ACK=$min(B,L+R\times RTT)$
## Pipelined protocols
Pipelined protocols allow multiple unacknowledged packets in the pipeline 
- ACK is sent **individually** or **cumulatively** 
- Range of sequence numbers must be **increased** 
- Two generic protocols: **Go-Back-N**, **Selective Repeat**

![[Pasted image 20231115093318.png]]

**TCP** is a combination of the two
### Go-Back-N
Sender can **send** up to $N$ packets without waiting for ACK
- $N$ = send window size
- Depends on:
	- The delay-bandwidth product
	- Receive buffer size
The receiver **maintains** a variable `expectedseqnum` which **keeps track** of the **next** expected sequence **number** to be received
- If the receiver correctly receives packet $n$ and $n$ = `expectedseqnum` then it sends ACK($n$) which acknowledges all packets up to and including packet $n$. **Cumulative ACK**
- Increments `expectedseqnum` by 1
- In all other cases, i.e., 𝑛 ≠ `expectedseqnum`, the receiver **discards** incoming packet, and sends ACK(`expectedseqnum`-1)

#### In action:
![[Pasted image 20231115094003.png]]

Inefficiency of the protocol:
- Once pkt2 timeouts, receiver has discarded everything else and so sender has to **resend** all of the other packets **again**

![[Pasted image 20231115094012.png]]

Here ACK1 has **cumulative** acknowledgement so sender knows receiver received pkt0 despite ack0 being lost
### Selective Repeat (SR)
SR receiver does **not** discard out-of-order packets, as long as they fall inside the **receive window**
- ACKs are **individual**, not cumulative
- Sender selectively **retransmits** packets whose ACK did **not arrive**
	- Maintains a **timer** for **each** un-acked packet in its send window
- The SR sender does **not** have to **retransmit** out-of-order packets

![[Pasted image 20231115095444.png]]

![[Pasted image 20231115095453.png]]
## TCP Reliable Data Transfer
TCP uses a **combination** of *GBN* and *SR* protocols
- Like *GBM*, TCP uses **cumulative ACKs**
- Like *SR*, the TCP sender **only retransmits** the segment **causing timeout**
### TCP Sequence Numbers
Byte-oriented protocol:
- Each byte of data is **numbered**
- Sequence number of a transmitted TCP segment is the 'byte' number of the **first byte** of the segment

![[Pasted image 20231120092434.png]]

- TCP ACK number is the number of **next expected byte** from the other side
- TCP uses **cumulative ACKs**

![[Pasted image 20231120092716.png]]
### TCP Duplex communication
In TCP, data can flow in **both directions**
- To **reduce** the number of transmissions, TCP piggybacks ACKs on data segments
- A segment can **carry data** and **serve as an ACK**

![[Pasted image 20231120092837.png]]
### Examples:

![[Pasted image 20231120093256.png]]

![[Pasted image 20231120093303.png]]
### TCP Fast Retransmit
The time-out period is often relatively **long**:
- Long delay before resending lost packet
**TCP Fast Retransmit**:
- Duplicate ACKs are good indicators of **packet loss**
- They are sent when there is a **gap** in **received stream of bytes**
Upon receiving 3 duplicate ACKs for a segment, TCP sender retransmits that segment **without** waiting for timeout

![[Pasted image 20231120094727.png]]
### TCP segment  structure

![[Pasted image 20231120094749.png]]
## TCP Flow Control
The data in the pipeline should **not exceed** the **receive buffer size**
- Otherwise, the receive buffer will **overflow** and data will be **lost**
**Flow control** tries to **adjust** the sending speed of the sender according to the **space available** at the receive buffer

How does the receive know the available space in the receive buffer?
- Receiver 'advertises' free buffer space in the **receive window field**
	- Denoted by `rwnd`
	- ![[Pasted image 20231120095054.png]]
- Sender **limits** amount of un-acked ('in-flight') **data** to receiver's `rwnd` value
- `W = lastByteSent - lastByteAcked <= rwnd`
- Guarantees receive buffer will **not overflow**

![[Pasted image 20231120095226.png]]
## Congestion Control
TCP provides **congestion control**
- Controls the **rate of transmission** according to the level of **perceived congestion**

**Congestion** at a router occurs when *input rate > output rate*

![[Pasted image 20231121140810.png]]

**Manifestations** (symptoms):
- **Lost packets**
	- Due to buffer overflow
- **Long delays**
	- Due to queueing in router buffers

**Key cause**: Senders sending packets **too fast**

**Goals of congestion control**:
Control the rate of the senders such that:
- Congestion does not occur in the network
- Each flow gets a 'fair' share of the network resources
#### Cost of congestion: Delay

![[Pasted image 20231121140843.png]]

When $\lambda$ approaches $C/2$, input approaches output and so queues build up
### TCP Congestion Control
#### Detection
TCP detects network congestion through **losses** and **delays**
A TCP sender assumes the network has been congested when:
- **Timeout** occurs
- **Three duplicate ACKs** are received
TCP treats these events differently
- A **timeout event** is taken **more seriously** than the reception of **3 duplicate ACKs**
#### What to control?
Remember: TCP is a window based pipelined protocol

If **window size** is $W$ bytes, then the **rate of transmission** is around $\frac{W}{\text{RTT}}$
- **Controlling** $W$ **controls** the rate of transmission

![[Pasted image 20231121142246.png]]

The maximum size of a TCP segment is called **Maximum Segment Size (MSS)**
- Determined by the **maximum frame size** specified by the **link layer** protocol
The number of segments to transmit **all data** in the window is $\lceil\frac{W}{\text{MSS}}\rceil$
#### How to control?
Sender maintains **congestion window size** denoted by `cnwd` and `W = LastByteSent - LastByteAcked <= min(cwnd, rwnd)`
- When `rwnd` is large, sender sends `cwnd` which determines the rate of transmissions
	- `rwnd` is the receiver window size - controls the amount of data received based on capacity of receiver
- Rate is about $\frac{\text{cwnd}}{\text{RTT}}$bytes/sec
- `cwnd` is dynamic
	- **function** of perceived network congestion
### AIMD
This varies `cwnd`
**Idea**: 
- Sender linearly increases transmission rate (`cwnd`) until loss occurs
- If loss occurs, **reduce** rate by a factor of **2**

*Additive increase*: **Increase** `cwnd` by **1** MSS every RTT until loss detected
*Multiplicative decrease*: **Cut** `cwnd` in **half** after loss

![[Pasted image 20231121143034.png]]

AIMD achieves '**fair**' rate allocation among competing flows

**Fairness goal**: 2 TCP senders share **same bottleneck link** of capacity $R$ bps, each should have a throughput (rate) of $R/2$

![[Pasted image 20231121143759.png]]

AIMD is **fair** because:
Two competing sessions:
- Additive increase gives **slope** of 1, as throughput increases
- Multiplicative decrease decreases throughput **proportionally**

![[Pasted image 20231121143854.png]]
### TCP slow start
Convergence speed of AIMD is **low**

To avoid waiting for long before getting the desired rate:
- TCP uses an initial **slow start phase** where window size is increased **exponentially fast** starting at a small value (slow start)
	- Slow start continues until a predefined threshold (`ssthresh`) is reached or loss detected
- This initial aggressive behaviour **ensures** that the sender reaches the right operating speed **quickly**

**Slow start**: Initially `cwnd=1MSS`, then **double** each RTT until a loss is detected or slow start threshold is reached
- Once `ssthresh` is reached, start AIMD
	- Increase `cwnd` by 1MSS each RTT
- `ssthresh` remembers the previous window size for which a loss occurred

![[Pasted image 20231121144647.png]]
#### Reacting to losses: Timeout and Duplicate ACKs
Losses are detected through **timeouts** and 3 duplicate ACKs
- **Duplicate ACKs**: Some segments are lost but some are received
	- Be **lenient**
	- `ssthresh=0.5 cwnd, cwnd=½cwnd` and window then grows linearly
- **Timeout**: No segment is received.
	- Take **drastic measure
	- `ssthresh=0.5cwnd, cwnd=1MSS`
	- After that the sender enters the **slow start** phase

![[Pasted image 20231121145217.png]]

