### Different kinds of NAT
There are mainly 4 kinds of NAT (Network Address Translation) techniques:

* SNAT (Static NAT)
* DNAT (Dynamic NAT)
* PAT (Port Address Translation)

### SNAT
SNAT (or Static Network Address Translation) is where all private IPs used within a network are mapped to a single public IP which is accessible via the internet. It is more used in the context of incoming traffic.

### DNAT
DNAT (or Dynamic Network Address Translation) is where there is a pool of public IP addresses called ‘NAT pool’ and the private IPs are translated to one of the public IPs in this pool. This is used more in the context of outgoing traffic.

### PAT
PAT (or Port Address Translation) is where both the IP address and the port of internal network connection (i.e. a private IP and port) is mapped to a public IP and another port before sending it to the internet. This makes the session of each connection involving different private IP but the same destination port (like multiple users in a network accessing the same webpage on the internet) to be unique.

PAT is what makes numerous users connect to the internet without exhaustion of the public IPv4 address space.

### Reference
https://www.youtube.com/watch?v=wg8Hosr20yw
