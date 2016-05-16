## Network Programming

##### 11.1 The Client-Server Programming Model

> An application consists of a **server process** and one or more **client processes**.
> A server manages some resource, and it provides some service for its clients by manipulating that resource.
> The fundamental operation in the client-server model is the transaction.

##### 11.2 Networks

> The a host, a network is just another I/O device that serves as a soutce and sink for data.

Network -> Network adapter -> Expansion slots - I/O bus -> I/O bridge

Network messages transform into standard I/O messages. (How?)

(What is a typical I/O device?)

> A network is a hierarchical system that is organized by geographical proximity.


**LAN**(Local Area Network): spans a building or a campus.
**Ethernet** is the most popular **LAN** technology.
An **Ethernet** segment consists of some **wires** and a small box called a **hub**.
Each wire has the **same maximum bit bandwidth**.
One end is attached to an **adapter on a host**, and the other end is attached to a **port on the hub**.
A **hub** slavishly copies every bit that is receives on each port to every other port.
Each **Ethernet** adapter has a globally unique 48-bit address that is stored in a non-volatile memory on the adapter.
Every host adapter sees the frame(the chunk of bits sent by a host), but only the destination host actually reads it.







