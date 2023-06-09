# Server establishes connections

The process is known as the TCP three-way handshake, and it's used to establish a TCP connection between a client and a server:

`SYN`: The client sends a TCP packet with the SYN (synchronize) flag set to the server. This packet includes a random sequence number (let's call it X) that the client chooses.

`SYN-ACK`: The server responds with a packet that has both the SYN and ACK (acknowledge) flags set. The ACK part acknowledges the client's SYN by including a number that's one more than the client's sequence number (X+1). The SYN part includes a random sequence number from the server (let's call it Y).

`ACK`: The client sends an ACK packet back to the server. This packet's acknowledgment number is one more than the server's sequence number (Y+1).

At this point, both the client and server have acknowledged each other's SYN packets, and the TCP connection is established.

It's also worth mentioning the concept of SYN and ACK queues (or more specifically, the SYN backlog and the accept queue), as these are closely related to the process of establishing connections.

When a SYN packet is received by the server, it is placed in the SYN backlog. If a corresponding ACK for that SYN is received (i.e., step 3 of the handshake is completed), the connection is then moved to the accept queue, where it waits until the application is ready to accept the connection.

This queuing mechanism allows a server to handle many incoming connection requests at once, even if the application isn't ready to accept them all immediately. However, if too many connection requests come in at once, the queues can fill up, and additional requests will be rejected until space frees up.
