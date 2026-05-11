Protocols specify the **structure** of internet packets
##### Application layer - Generates the message

![[Pasted image 20231029193003.png]]
##### Transport layer - Adds transport layer header

![[Pasted image 20231029193027.png]]

![[Pasted image 20231029200807.png]]

- e.g. if packet is a SYN packet then SYN bit will be 1
##### Network layer - Adds network layer header

![[Pasted image 20231029193047.png]]

![[Pasted image 20231029200612.png]]

**Length** of header can vary due to **optional** fields
##### Link layer - Adds link layer header

![[Pasted image 20231029193206.png]]

![[Pasted image 20231029200117.png]]

- When a packet is received, this is the **first layer** at which the receiver **starts** **reading** it
##### Physical layer - Transmits the packet

![[Pasted image 20231029193237.png]]

- Packet is **transmitted** via the network (physical layer) once **all headers added**
- Once delivered, packet is received by receiver