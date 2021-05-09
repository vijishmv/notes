### Tunneling
In Construction, a tunnel is a structure that helps to connect two areas which are otherwise not accessible. Imagine two cities separated by a mountain range, a tunnel through these mountains would make the cities connect with each other.

Similarly, in networking, a tunnel is a concept that makes two networks communicate which is otherwise not possible or not permitted using traditional networking infrastructure.

Some of the reasons why the communication is limited are:

### Security
The traffic between these two networks shouldn’t be visible to the public internet

### Latency
The packets need to be delivered fast like gaming, video conferencing etc.

### Other technical limitation
Both the networks use IPv4 addressing while the infrastructure between them only permits IPv6 addressing

Tunneling makes use of certain protocols that help mitigate these concerns by encapsulating the source packets (dividing the source packet and adding protocol specific headers). These are then send through the native network infrastructure just like any other packet.

Some well known tunneling protocols are:

### IPsec (Internet Protocol Security) Protocol Suite
This is used for encrypting the traffic between the networks. It is used in VPN technology

### GRE (Generic Routing Encapsulation)
This protocol encapsulates packets that use one kind of routing protocol inside the packets of another protocol. It sets up a point to point connection across a network (i.e. multiple hops would turn into a single hop). GRE adds two headers. The GRE header specifies the GRE specific protocol used. The IP header encapsulates the original packet’s IP header and payload.

### VXLAN (Virtual Extensible LAN)
This protocol helps solve the scalability problem (IP address space limitation) associated with large cloud computing deployments. It uses a VLAN-like encapsulation technique to encapsulate layer 2 Ethernet frames within layer 4 UDP datagrams, using 4789 port.
