### tshark

Tshark is the CLI form of wirshark. If wireshark is installed, then tshark binary is available in the same.

The rpm package to install tshark is wireshark-cli.

Tshark displays the same output as wireshark. It can also display package information with the same colour coding as used in wireshark. This is achieved with --color option.

Simple syntax to view packets:

```tshark --color -i <interface>```

To write to a file:

```tshark -i <interface> -w <file.pcap>```

Here all are written to the file but you cannot see what packets are being captured.

To see the packets also while writing to file, use ``` -P```

To disable name resolution use ```-n```

To stop capture after a specific number of packets use ``` -c <number> ```

To capture specific packets based on  *capture filter* use ```-f <filter rule>```. While using this, only the respective packets are displayed and written to file:

```tshark -i <interface> -w <file.pcap> -f udp```

List of avilable capture filter can be determined from ```man pcap-filter```

Some of the most common capture filters are:
```
host <x.x.x.x>
src host ...

net <x.x.x.x/24> or net <x.x.x.x> mask 255.255.255.0 
src net ...
dst net ...

ether host MAC:MAC:MAC:MAC:MAC:MAC
vlan
ip
port <x>
tcp portrange <x>-<y>

```

To read from a pcap file:

```tshark --color -r  <file.pcap>```

To filter out packets matching a specific *display filter* use ```-Y <filter rule>```. Note that -f and -Y have difference in the rules they accept. 

```tshark --color -r  <file.pcap> -Y http```

List of all display filters - https://www.wireshark.org/docs/dfref/ 

Some of the most common capture filters are:

```
ip.addr==<x.x.x.x>
ip.addr==<x.x.x.0>/24
ip.addr==<x.x.x.x>  && ip.addr==<y.y.y.y>

tcp.port==<x>
tcp.flags==<x>

!(arp or icmp)
```
To list all interfaces that tshark can listen on:

```tshark -D```


To see the entire packet contents, use ``` -V```

```tshark --color -i <interface> -V```

To see the packet contents of only specific protocols while reading pcap file or capturing, use -O <protocol>:

```tshark --color -i <interface> -O http```

