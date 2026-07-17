"Supernetting" - aggregating together multiple class C addresses by using masks to summarise them as one network.
    - Not a great idea to do as it can confuse routing protocols (first 3 octets should never change)
    - Likely to come across these in existing, established networks
    - Cheaper than buying a class B domain
    - Obsolete in private networks

Conceptual network diagrams may show switches with a high number of hosts (1200, 500, 2200 etc) - this is not representing one switch directly - no switch exists with that many ports. It represents a cluster/stack of Level3 & Level2 switches which will handle addressing via DHCP

Networks must be reliable - avoid single point of failure that would bring the network down entirely.
Resilience:
- Switches arranged in switch blocks to increase reliability
- Super-trunks between switches called Etherchannel
- Created using "backbone cables" (also vulnerable to going down (being disconnected, accidentally severed etc))

Etherchannel:
- Set up of 8 ports configured to be one single trunk
- If a cable breaks all the others in the channel take over
- Also load balance to spread the traffic load (efficiency goes up)
- Port Aggregation Protocol (PAgP) - Cisco Proprietary - not suitable for mixed-vender switches, requires all Cisco hardware (expensive)
- Link Aggregation Protocol (LACP) - Open Standard, 802.3ad

Router Redundancy:
- Hot Standby Routing Protocol (HSRP)
    - Utilise virtual default gateways - means connected clients do not lose connectivity to the outside world even when a specific router fails, traffic is re-routed to the new router using the same virtual default gateway
    - Address distributed to client machines via DHCP
    - All interfaces that face client machines will have standby settings on them:
        - Best router of the set will be marked as the primary
- Kicks in when connectivity to a single router is lost
- Used to get a set of routers (2+) to act as if they are one combined router
- Desirable routers are set up via priority (fail to a specific model first, typically better hardware)
- Not very efficient vs GLBP (Gateway Load Balancing Protocol)

Automatic Address Allocation:
- DHCP can be manipulated to give one specific machine one specific address
    - Useful for targetted server IP addressing
    - Redundancy servers etc
- Desirable as it takes out the hard work of manually assigning IPs to thousands of devices
- DHCP servers dynamically assign or lease addresses from a permitted pool of addresses:
    - Either for a set lease time
    - Or granted permanently until the host is powered off or moved to another network or subnet
- Dynamic Host Control Protocol (DHCP)
    - Server based utility that responds to requests for partial automatic configuration of a network host on request
    - DHCP on IPv6 networks is almost obsolete as IPv6 systems use the more efficient SLAAC system to get network hosts to automatically generate their own addresses
- DHCP Process
    - New unit is switched on and broascats a DHCPDISCOVER message with its own MAC address to discover available DHCP servers
        - Fastest DHCP server "wins"
    - When a DHCP server receives this message:
        - IP automatically reserved for the client
        - ARP entry with the MAC address of the client and its reserved address
        - Issues a binding DHCPOFFER message to the requesting host by a layer 2 unicast reply using the source MAC address of the server and the destination MAC address of the host
    - Client receives DHCPOFFER message
    - Client replies with a unicast DHCPREQUEST message - taken as binding acceptance of the parameters based on which was the fastest and closest reply
    - Server verifies lease information to ensure host is still there via ICMP ping
    - Creates new ARP entry for client release
    - Replies with unicast DHCPACK (acknowledgement)
    - Client receives DHCPACK it apots new IP config and performs ARP lookup with new address, if no reply to the ARP lookup the client knows it has an address which is valid and does not belong to anyone else
    - ARP process ensures that duplicate addresses don't exist on the same network segment
- Configuration of DHCP for IPv4:
    - It can hand out its own address if not configured
    - `ip dhcp excluded-address <start IP>|[<end IP>]`
    - Used for stopping DHCP from giving out specific IPs, can be a single IP or in a range
    - Always configure excluded IPs first - may have handed out its own address if the pool is set up before the exclusions
    - Configure addresses it can hand out:
        - `ip dhcp pool <poolname>` - ex `ip dhcp pool fred`
        - `network <network ID/subnet ID> <mask>` - ex `network 10.0.0.0 255.255.255.0`
        - `default-router <default gateway address>` - ex `default router 10.0.0.1`
        - `exit`
        - Using the example commands DHCP would hand out IP addresses between 10.0.0.1 and 10.0.0.254
- Configuration of DHCP for IPv6:
    - DHCP all but completely redundant
    - SLAAC - StateLess Address Auto Configuration
    - Easy to implement
    - `ipv6 enable` command to interface configuration on the router which is connected to the network needing automatic addresses
        - Device needing an address will create its own address
    - Make sure that the only mask used is `/64` for it to work

