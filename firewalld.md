Firewalld is a frontend to nftables. It provides a simple cli tool - **firewall-cmd** and a a GUI tool - **firewall-config**.

Firewalld should be used for simple scenarios while nftables should be used for complex and performance related firewall settings.

Firewalld rules are translated to nftable rules while nftable rules are not rewritten back to firewalld configs.

Changes made via firewall-cmd don't take effect without executing `firewall-cmd --reload`

To save the changes and apply the same even after reboot `firewall-cmd --permanent`

