## firewalld

Firewalld is a frontend to nftables. It provides a simple cli tool - **firewall-cmd** and a a GUI tool - **firewall-config**.

Firewalld should be used for simple scenarios while nftables should be used for complex and performance related firewall settings.

Firewalld rules are translated to nftable rules while nftable rules are not rewritten back to firewalld configs.

The systemd service is **/usr/lib/systemd/system/firewalld.service** .

Firewalld introduces the concept of **zones**. firewalld categorizes incoming traffic into zones defined by the source IP and/or network interface. Each zone has its own configuration to accept or deny packets based on specified criteria.

Firewalld makes it easier to specify services by using the name of the service rather than its port(s) and protocol(s). For example, samba rather than UDP ports 137 and 138 and TCP ports 139 and 445.

The top layer of organization in firewalld is zones. Then comes **tables** (which is based on a family)  and finally the **chain**. Unlike iptables/ip6tables, tables and chains are not pre-created. 

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

Changes made via firewall-cmd don't take effect without executing `firewall-cmd --reload`.

To save the changes and apply the same even after reboot `firewall-cmd --permanent`.


