## firewalld

Firewalld is a frontend to nftables. It provides a simple cli tool - **firewall-cmd** and a a GUI tool - **firewall-config**.

Firewalld should be used for simple scenarios while nftables should be used for complex and performance related firewall settings.

Firewalld rules are translated to nftable rules while nftable rules are not rewritten back to firewalld configs.

The systemd service is **/usr/lib/systemd/system/firewalld.service** .

Firewalld introduces the concept of **zones**. firewalld categorizes traffic into zones. Broadly there can be 2 types of zones (or a mix of these):
1. A source IP range, also called **source zone**
2. A network interface, also called **interface zone**
Each zone has its own configuration to accept or deny packets based on specified criteria.

Source zones have precedence over interface zones.

Firewalld makes it easier to specify services by using the name of the service rather than its port(s) and protocol(s). For example, samba rather than UDP ports 137 and 138 and TCP ports 139 and 445.

The top layer of organization in firewalld is zones. Then comes **tables** (which is based on a family)  and finally the **chain**. Unlike iptables/ip6tables, tables and chains are not pre-created. 

For every zone, you can set a behavior that handles incoming traffic that is not further specified. This is called **target** of the zone. These are - default, ACCEPT, REJECT, and DROP.
 
ACCEPT -  accepts all incoming packets except those disabled by a specific rule. 
DROP - dumps the packet and does not notify the sender.
REJECT - will dump the packet and also notify the sender regarding the rejection of the packet.

### Check regarding the below
The **default** target is the same as reject.  If you have a zone that has a target of default, It is recommended to change it to one of REJECT, or DROP.


There are several predefined zones available. To list them:
```
# firewall-cmd --get-zones
block dmz drop external home internal public trusted work
#
```

The zones that are active currently is determined with:
```
# firewall-cmd --get-active-zones
public
  interfaces: enp0s3
#
```

To open port 80 on the public zone:
```
# firewall-cmd --zone=public --add-service=http
 success
#

# To do this using port

# firewall-cmd --zone=public --add-port=80/tcp
 success
#
```

Note port has to be specified along with the protocol - 80/tcp. 

To close a port or service (i.e close it in the firewall):
```
# firewall-cmd --zone=public --remove-port=80/tcp
 success
#

# firewall-cmd --zone=public --remove-service=http
 success
#
```

To use a different zone as your default:
```
# firewall-cmd --set-default-zone=home
 success
#
```

# Check if the below works:
To create a new zone with description:
```
# firewall-cmd --permanent --zone=OAM --set-description="OAM network zone"
 success
#
```
Note, the description is optional



To list all the firewall rules in use currently:
```
# firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3
  sources:
  services: cockpit dhcpv6-client ssh
  ports:
  protocols:
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:

#
```

The first line indicates the zones that are active currently. 


The sources line, lists the IP range accepted in the zone

The services line indicates the services that are permitted by the firewall. These are the predefined services available in /usr/lib/firewalld/services/ . If a service is not availble, it can be created as an xml file and placed in /usr/lib/firewalld/services/. Or the necessary ports used by the service can be specified which will be thus mentioned in the **ports** line. 

**masquerade** line indicates whether, **IP Forwarding** (i.e. this machine acts like a router) is enabled.

**forward-ports** defines the source and destination ports that are forwarded within this machine. 

Check the below:

**icmp-blocks** defines the IP ranges that are blocked from responding to ping (ICMP).

**rich rules** defines advanced firewall rules 


if a situation arises where you need to open a temporary hole in your firewall (perhaps for ssh), you can add the service to just the current session (omit --permanent) and instruct firewalld to revert the modification after a specified amount of time:
```
# firewall-cmd --add-port=75/tcp --timeout=10s
success
# 
```

If needed, all changes can be made in the runtime and then later make them permanent by finally running: 
```
# firewall-cmd --runtime-to-permanent
success
#
```

To save the changes and apply the same even after reboot use `firewall-cmd --permanent` in the command. 

If changes are made to the permanent configuration (i.e. the commands you ran had --permanent) and you want it to be reflected in the live:
```
# firewall-cmd --reload
success
#
```



### References
https://fedoraproject.org/wiki/Firewalld

https://firewalld.org/documentation/

https://www.putorius.net/introduction-to-firewalld-basics.html

https://www.linuxjournal.com/content/understanding-firewalld-multi-zone-configurations



