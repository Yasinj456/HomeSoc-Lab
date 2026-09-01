I started by setting up the three VMs for the SOC lab: Kali, Windows, and Debian. I put all three on UTM's Host Only Network 0 so they could communicate with each other on an isolated network.

The IPs I ended up using are:

Kali: 192.168.50.10
Windows: 192.168.50.20
Debian/Wazuh: 192.168.50.30

I used static IPs because UTM wasn't providing DHCP for this network.

The setup took longer than I expected because the VMs initially couldn't communicate consistently. I checked the network interfaces, routing table, and ip neigh on Debian to see whether the problem was with the interface, routing, or ARP.

Eventually I found that Windows was blocking inbound ICMP. The network itself was working, but Kali could not ping Windows because the Windows Firewall ICMPv4 Echo Request rule was disabled. I enabled the appropriate inbound rule, and Kali was then able to ping Windows normally.

After fixing that, I tested the machines again and confirmed that Kali, Windows, and Debian could communicate with each other with 0% packet loss.

This was a good reminder for me not to immediately assume the virtual network is broken when something cannot ping. Checking the interface and routes of each machine individually helped narrow the problem down to the Windows firewall.

The network is now set up for the rest of the lab, with Kali generating activity, Windows producing endpoint logs, and Debian running Wazuh. 