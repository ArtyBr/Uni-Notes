## SYN Attack

![[Pasted image 20231101093359.png]]

[[Non-persistent HTTP (v1.0)]]

![[Pasted image 20231101093623.png]]

- **Creating** half open connections means **allocating resources** - *buffer, memory* etc.
- **Exhausts** the server by just creating these half-open connections
- When server sends back syn-ack packets, attacker **doesn't respond** and **holds up** the **resources** of the server
	- Normal user is **denied from service** by server due to **lack of resources**
- This is a **denial of service** attack
## ARP Cache Poisoning attack
### How do MAC Addresses work?
- Src and dest *IP addresses* remain the **same** through the process of sending a packet
- *MAC addresses* identify network interfaces - **Link layer** address
	- Src and dest *MAC addresses* change in **each hop**

![[Pasted image 20231101094353.png]]

### Address Resolution Protocol (ARP)
Link layer **needs the MAC address** in order to build the link layer frame
ARP tells us how we get the MAC address for the **next hop**

To find the MAC address corresponding to an IP address:
- The router **broadcasts** an **ARP request packet** to **all interfaces** on-link with the **sending interface**

![[Pasted image 20231101094923.png]]

- ARP request packet is broadcast to all interfaces on link with the sending interface

![[Pasted image 20231101095208.png]]

- ARP **reply** is sent by the node **having** the **requested address**
- IP address to MAC address **mapping** is **stored** in an **ARP cache** for future use

![[Pasted image 20231101095308.png]]
### ARP Cache Poisoning
- ARP allows **unsolicited replies** since IP addresses **can change**
- An **attacker** can send an **unsolicited** ARP reply **pretending** to be **someone else**
- The **recipient** will then send all messages **meant for R2** to the **attacker**

![[Pasted image 20231101095504.png]]

