**Processes** running on different **host machines** can communicate by sending messages over a **network**
- Communicating processes form a **network application**

- When developing a network application, a developer generally has to develop both the **client** side and the **server** side of the program
- Developing one side of the program is enough for applications specified by **standard protocols** 
	- e.g. *Web (HTTP), Email (SMTP)*
	- Both sides are developing using the **same set of rules**
##### Examples
![[Pasted image 20231023081857.png]]

**Web** - Processes (**client**) sends requests for a web page to another (**server**) process, which would respond to the request by sending the requested file
**P2P** - One peer (**client**) requests file from another (**server**) and other responds by sending requested file
#### Sockets
Processes **send/receive** messages via **sockets** (analogous to doors) 
- Sockets are APIs between the **application** and **transport** layers
- Whenever the sender has a message to send, it **creates** a socket and **writes** the message onto the socket with proper **addressing** **info**. The layers below and the internet is responsible for **carrying** the message to the **receiving** process 
- The receiver has another **socket** into which the message is **written**. The receiving process simply **reads** the message from the **socket**.

![[Pasted image 20231023082215.png]]

Message has to **pass through** **all** layers **down** to physical on the **client side** and has to travel **back up** the layers on the **server** side when sending a request
#### Addressing Processes
Messages need to be addressed to the **correct process** running within the **correct end host**
- Any internet device (**host**) can be **identified** by **IP addresses**
	- IPv4 addresses are 32 bit numbers written as dotted decimal, e.g., 154.31.16.13 (10011010 00011111 00010000 00001101)
- **Processes** can be identifies by **port numbers**
	- Port numbers are **16 bit numbers** ranging from 0 to 65535 (2 − 1) 
	- Port numbers 0 to 1023 are **reserved** for **well known** network applications
		- e.g., port 80 for HTTP, port 25 for SMTP, etc. 
	- Port numbers **above** 1023 can be used by **other** application programs
### Transport layer services
Application processes use **transport layer services**
- Transport layer is expected to **deliver** messages to their **intended recipients**
**All** transport layer protocols offer some basic **services**
- e.g. *packetization, addressing, sequencing, error correcting bits*
An app may require **additional services** from the transport layer
- e.g. *reliable* and *in-order* delivery of packets

Transport layer services are bundled into two packages:
#### TCP - Transmission Control Protocol
TCP offers:
- **Reliable** and **in-order** data transfer service
	- Packets can be lost or arrive out of order otherwise
		- Due to buffer queues in routers, when they are full the packets are lost due to **buffer overflow**
		- Due to the parallelization of sending packets, some packets may arrive **before** others out of order
- **Connection oriented service**
	- **Setup** required between client and server processes **before** they start **transferring data**
	- Called **TCP handshake**

![[Pasted image 20231023093554.png]]

Client sends a small packets with a special bit **syn=1** - sent to server just to request the **initiation** of a **new connection**
- After receiving the packet, the server will **allocate** some **resources** in order to **establish** the connection
	- **Buffer** to store out of order packets etc.
	- Reserves a **connection socket**
- Server **acknowledges** to the client that a connection has been **established** using **syn ack** packet sent back to client
	- **syn = 1, ack = 1**
- Time between client sending **syn packet** and receiving the **syn ack packet** is called the **round trip time (RTT)**, during which no data is sent or received
- Now the client can start **sending** **data** with the ack packet
Called **3-way** connection establishment service
#### UDP - User Datagram Protocol
UDP provides **no guarantees** on data transfer
- **Best effort service**
	- **Packetizes data** and **sends** it to the network
	- **No** effort is made to **recover losses**
	- Up to process itself to maintain guarantees if it wants
- **UDP is faster**
	- **No** connection **set up** is required, UDP **headers** are **smaller** than TCP headers
		- Applications such as *Skype, internet telephone, games*
### Building network applications
**Goal**: Learn how to build client/server applications that communicate using sockets
**Socket programming**: Create sockets, read from and write to sockets through system calls

![[Pasted image 20231023094810.png]]

**TCP**: Connection **oriented** sockets
**UDP**: Connection**less** sockets

#### Example: UDP application

![[Pasted image 20231023094914.png]]

##### Client
```c
/*
    send udp messages
    This sends a text message entered by a user
        usage:  udp-send
*/

#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <netdb.h>
#include <unistd.h>
#include <sys/socket.h>
#include <arpa/inet.h>

#define BUFLEN 2048
#define PORTNO 8888
  
int findlen(char *str){
    int i=0;

    while (str[i]!='\n'){
        i++;
    }
    return i;

}

int main(void)
    struct sockaddr_in remaddr;  // custom struct variable to hold addresses of the server
    int clientSocket;   // int file descriptor to represent client socket
    char buf[BUFLEN];   /* character array to store text messages */
    int len, recvlen;       /* track the lengths of messages */
    unsigned short port_num=PORTNO;
    
    /* create a UDP socket, if not successful, exit with error message.
    The first argument AF_INET of socket() specifies that the socket is going to use IPv4 addresses, the next argument SOCK_DGRAM specifies that the type of the socket is UDP, the last argument is for protocol family and is set to 0 in most applications. see the man page of socket(). */
	if ((clientSocket=socket(AF_INET, SOCK_DGRAM, 0))<0){
        printf("Error: Socket creation failed\n");
        exit(1);
    }
    /* now define remaddr, the address to whom we want to send messages. For convenience, the host address is expressed as a numeric IP address that we will convert to a binary format via inet_aton */

    remaddr.sin_family = AF_INET;
    remaddr.sin_port = htons(port_num); // this specifies the server port. htons() converts host byte order to network byte order
    remaddr.sin_addr.s_addr = inet_addr("127.0.0.1");  // this specifies the server's IP address

  /*  getting a message from the user */

    printf("Enter a message: ");
    fgets(buf,BUFLEN,stdin);
    len=findlen(buf);
    buf[len]='\0';

    /* see the man page sendto(). It sends data to remaddr. The data is first written from buf to the clientSocket which then tells the UDP to send the data to the address specified by remaddr.*/
    
    if (sendto(clientSocket, buf, strlen(buf), 0, (struct sockaddr *)&remaddr, sizeof(remaddr)) < 0) {
        printf("Error: sendto\n");
        exit(1);
    }
    printf("Sent message: %s\n",buf);
        /* now receive a message from the server */
    recvlen = recvfrom(clientSocket, buf, BUFLEN, 0, NULL, NULL);
  if (recvlen >= 0) {
          buf[recvlen] = '\0';  /* expect a printable string - terminate it */
          printf("received message: \"%s\"\n", buf);
  }
    close(clientSocket);
    return 0;
}
```
##### Server
```c
/*

        demo-udp-03: udp-recv: a simple udp server

    receive udp messages

  

        usage:  udp-recv

*/

  

#include <stdlib.h>

#include <stdio.h>

#include <string.h>

#include <netdb.h>

#include <sys/socket.h>  // required for the socket(), recvfrom(), sendto()

#include <arpa/inet.h>   // required for htons()

#include <ctype.h>

  

#define BUFSIZE 2048

#define PORTNO 8888

  

int main(int argc, char **argv)

{

    struct sockaddr_in myaddr;  /* struct variable to store the address of server */

    struct sockaddr_in remaddr; /* struct variable to store the address of client */

    socklen_t addrlen = sizeof(remaddr);        /* length of addresses */

    int recvlen;            /* # of bytes received */

    int servSocket;             /* file descriptor for the server socket */

    int msgcnt = 0;         /* count # of messages we received */

    char buf[BUFSIZE];  /* receive buffer to store the received message */

    int i;

    unsigned short port_num=(unsigned short) PORTNO;

  
  

    /* create a UDP socket, if not successful, exit with error message.

    The first argument AF_INET of socket() specifies that the socket

    is going to use IPv4 addresses, the next argument SOCK_DGRAM specifies

    that the type of the socket is UDP, the last argument is for protocol family

    and is set to 0 in most applications. see the man page of socket(). */

  

    if ((servSocket = socket(AF_INET, SOCK_DGRAM, 0)) < 0) {

        fprintf(stderr,"cannot create socket\n");

        exit(1);

    }

  

    /* Fill up the different fields of the struct variable myaddr with relevant information so

       that it can be used as a valid address for the server socket */

  

    myaddr.sin_family = AF_INET;    // this field sets the address type to IPv4

    myaddr.sin_addr.s_addr = inet_addr("127.0.0.1"); // this field sets the IP address, we use the IP address of the loopback interface, see the man page of inet_addr()

    myaddr.sin_port = htons(port_num);  // we use port number 2021, see the man page of htons()

  
  

    /* bind the socket to the specific IP address and port above. This gives the server

    a specific IP and port number which can be used by clients to

    connect to the server. Note that the socket() call assigns a random port number to the socket

    but we want to use a specific port number at the server, that's why bind() is required on the server side.*/

  

    if (bind(servSocket, (struct sockaddr *)&myaddr, sizeof(myaddr)) < 0) {

        printf("Error: bind failed\n");

        return 0;

    }

  

    /* now loop, receiving data and printing what we received */

    while (1) {

        printf("waiting on port %d\n", port_num);

  

        /*See the man page of recvfrom() system call. It receives data from the servSocket

         and writes it into the buf; at most BUFSIZE bytes are read at a time. The number of

         bytes actually read from servSocket is returned. After this call the struct remaddr

         is filled in with the source address of the message (in this case it is the client address).

         The call blocks the server process until some data is received*/

  

        recvlen = recvfrom(servSocket, buf, BUFSIZE, 0, (struct sockaddr *)&remaddr, &addrlen);

  

        if (recvlen > 0) {  // print the data if recvlen > 0

            buf[recvlen]='\0';

            printf("received message: \"%s\" (%d bytes)\n", buf, recvlen);

            printf("received message from IP:%s and port number: %i\n", inet_ntoa(remaddr.sin_addr),ntohs(remaddr.sin_port));

        }

        else

            printf("uh oh - error reading the message!\n");

  
  

        /* convert the received message to upper case */

        for (i=0;i<strlen(buf);i++){

            buf[i]=toupper(buf[i]);

        }

  

        /* see the man page sendto(). It sends data to remaddr. The data is first written from

        buf to the servSocket which then tells the UDP to send the data to the address specified by remaddr.*/

  

        if (sendto(servSocket, buf, strlen(buf), 0, (struct sockaddr *)&remaddr, sizeof(remaddr)) < 0){

            fprintf(stderr, "Error in sendto\n");

            exit(1);

        }

    }

    /* never exits */

}
```
## HTTP and the Web
**Web browsers** communicate with **web servers**
- **Both** processes use *HTTP* as the application layer **protocol**
- HTTP uses *TCP* and **port number** 80
#### A web page
- A web page consists of **objects**
- An object is a **file** e.g. *HTML* file, *JPEG* file, *Java applet* etc.
	- **Stored** in the **web server**
- Each object is **addressable** by a *URL*
	![[Pasted image 20231024143724.png]]
- Most pages consist of a **base HTML file** and **several referenced objects**
### HTTP
- Client program sends HTTP request messages to **request** a web page
- The server **responds** with an HTTP response message containing the requested web page
- **Structure** of the messages and their **sequence** are specified by HTTP

*HTTP* uses *TCP*
- The server **runs** on the server process on port 80
- Client **initiates** TCP connection to a server, port 80
- TCP connection is **established** after a TCP **handshake**
- HTTP messages are **exchanged**
- TCP connection is **closed**

Two versions of HTTP: *HTTP v1.0 (non-persistent)*, *HTTP v1.1 (persistent)*
- **Non-persistent (v1.0)** - Each object is obtained over a **separate** TCP connection. Downloading **multiple** objects will require **multiple** TCP connections
- **Persistent (v1.1)** - **Multiple** objects can be sent over a **single** TCP connection
#### Non-persistent HTTP (v1.0)
Steps needed to download a webpage containing 1 **base HTML file** and 10 **referenced images**:
- The **clients** sends *SYN*, the **server** **responds** with *SYN-ACK*
- **Client** sends *ACK* and **adds request** for the base HTML file
- The **server** establishes the connection and **responds** with the base HTML file
- HTTP server **closes** TCP connection
- Client **receives** the HTML file and examines it to find 10 other referenced objects
- Steps 1-4 are **repeated** for **each** of the 10 referenced files
##### Response time:
- **RTT**: Round Trip Time
- 1 RTT to **initiate** TCP connection
- 1 RTT to send HTTP request and receive first few bytes of the requested object
- File transmission time
- Response time = 2[RTT] + [File transmission time]

![[Pasted image 20231024141350.png]]
##### Problems with Non-persistent HTTP
- Each object requires **at least 2RTT** to be downloaded
- For each TCP connection the OS of the server has to **allocate some resources** (buffers, local variables etc.)
	- Problematic when the server has to handle a **large number** of **requests**
#### Persistent HTTP (v1.1)
- Server leaves connection **open** after **sending response**
- **Subsequent** HTTP messages are exchanged over the **open** connection
- Client **sends requests** back-to-back as soon as it **encounters** a **referenced object**
- **All** objects **downloaded** within 2[RTT] + [Total data transfer time]
### HTTP Request and Response message
Two types of HTTP messages: **Request** and **Response**
- Written in ASCII, human readable
#### Request

![[Pasted image 20231024144654.png]]

**General format:**

![[Pasted image 20231024144730.png]]
#### Response

![[Pasted image 20231024144908.png]]
### Web cache (proxy server)
**Goal**: Satisfy client request **without** involving **origin server**
- Web clients can be configured to access the web via a **web cache**
- Browser sends all HTTP requests to **cache**
	- Object in cache: cache **returns** object
	- Else cache **requests** object from **origin server**, then **returns** object to **client**

![[Pasted image 20231024145422.png]]

Typically cache is installed by ISPs to **serve** the requests of **popular content**

- Reduces **response time** for client request
- Reduces **traffic** going out of an ISP network
- Reduces the **cost** for the ISP
- Reduces the **traffic** for the **internet as a whole**

