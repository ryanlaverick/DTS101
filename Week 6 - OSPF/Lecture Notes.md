# OSPF - Open Shortest Path First
v1 - IPv4 (outdated) - can still be encountered in the wild, can't fully work with v2/v3
v2 - IPv4
v3 - IPv6

Requires decent amounts of system/network resources
Link state routing protocol
Capable of handling very large networks

Metrics for routing protocols:
- RIP - uses hop count
- EIGRP - uses a complex metric based on bandwidth, delay, load and reliability
- OSPF - cumulative bandwidth

Every router within an OSPF network will build a "full scale" network picture - requires routers with more resource/better hardware

AD - Administrative Distance, used on a multi-protocol routing device (RIP + EIGRP, EIGRP + OSPF, RIP + OSPF etc)
- Smallest number wins (more preferred and the "better" the route is)
90 - EIGRP
110 - OSPF
120 - RIP

Direct Connection: 0
Static/Default Route: 1
EIGRP Summarised Route: 5
External Border Gateway Protocol (eBGP) - 20
Internal Border Gateway (iBGP) - 90
OSPF - 110
RIP - 120

DR - Designated Router (primary)
BDR - Backup Designated Router (secondary)
Grunts - all others

Multicast messages for topology updates are sent to DR/BDR on 224.0.0.6 and receive messages from DR/BDR 224.0.0.5

DR/BDR are elected by routers in a network
Can be influenced by setting one of these values below so that the desired device becomes DR/BDR:
- Highest value router ID - specified in dot decimal notation like an IP address when OSPF is configured
- If no router IDs set, then highest IP address value on the particular router wins. Adding loopback interfaces with higher IP addresses on them can be used to set this up (using /32 mask - 255.255.255.255)
- Or the highest OSPF priority level set wins (255 - always win, 0 - never win)

Wildcard masks - used when setting up OSPF, inversion of ordinary masks
255.255.255.0 becomes 0.0.0.255
255.255.255.128 becomes 0.0.0.127
255.255.255.192 becomes 0.0.0.63
Convert all 1's to 0's
Convert all 0's to 1's

Masks are always 0's or odd numbers. Even numbers are not valid and are caused by an incorrect calculation.