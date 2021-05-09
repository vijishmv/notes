### Switch

A switch is a layer 2 device, It knows how to work with ‘frames’ only. It keeps a table called MAC table where it records the MAC addresses and the port where this MAC address is found. This table is populated automatically i.e. you don’t have to create or update this table. Each of the interface in a switch (unlike a router) doesn’t have an IP associated with it.

Basically a switch is considered as a mix of a hub and a repeater/bridge.

A Repeater is a two port device which works in Layer 1 (physical) to amplify the input signals and output them through the other port. So any signal at one port is send blindly to the other port.

A Bridge is like a repeater having 2 ports but it works at layer 2. The major difference is that it permits only those frames meant for the destination to pass through. This prevents flooding of packets in the other network segment. Bridges were traditionally used to connect two network segments which otherwise used different Layer1 implementations.

A Hub is a multiport repeater (so it is a physical layer device). It returns any signal it gets at one port to all the other ports unconditionally.

A switch works like a hub + repeater/bridge. It gets frames at one port and sends them out only through that specific port where the destination MAC for that frame resides. It prevents flooding (unlike a hub).

Switching is the only activity that happens in the same network (i.e. here the subnet of each machine connected to the switch is the same and they use only MAC to communicate).

Switching happens at every hop, i.e. the MAC address in the L2 frame changes every time the packet reaches a switch. This is not the case with routing. In routing the IP of the packet remains unchanged through out the end to end transaction of the packet.

### Router
A Router is a layer three device. It is an intelligent device which is aware of the IP of the packets traversing through it. A router retains a table knows as ‘routing table‘ consisting of routes. These must be pre-configured in the router (Manual router configuration). This is the normal case.

The is also an option to configure the router automatically by using protocols like BGP which is how the internet works.

Each of the interface in a router (unlike a switch) has an associated IP and MAC. Each of theses IPs (on separate interfaces) will be from a different subnet.

The routing table is what enables the router to transfer packets from one subnet to the other. There is also a special route called the ‘default gateway route‘ that determines what to do to a packet if it does not match any of the routes in its routing table.

### References
https://www.youtube.com/watch?v=H7-NR3Q3BeI

