# VLANs
## Legacy Inter-VLAN Routing
- Equipment:
    - Routers: 2621XM, 1841 or 2811 series
    - Switches: 2960 or 2950 series
    - PCs
- Cable up network from diagram - take care to ensure cables are in correct ports. Best to do PCs -> Switch first and then Switch -> Router so VLAN ports do not have their ports overtaken

Example:

![alt text](legacy-inter-vlan.png)

### Switch Config
- `enable`
- `configure terminal`
- `hostname <name>` - eg `hostname s1`
- `vlan <number>` - eg `vlan 10`
- `name <name` - eg `name Sales`
- configure other VLANs if required
- `interface range port-port` - eg `interface range fa0/1-12`
- `switchport mode access`
- `switchport access vlan <number>` - eg `switchport access vlan 10`
- `no shut`
- `exit`

Debug using `show vlan` (or `do show vlan` if in privileged mode) - you should see the port ranges appearing under the VLAN

### Router Config
- `enable`
- `configure terminal`
- `hostname <name>` - eg `hostname r1`
- configure receiving ports (on diagram this is fa0/0, fa0/1)
- `interface <port>` - eg `interface fa0/0`
- `ip address <ip address> <mask address>` - for VLAN10 in this diagram this would be `ip address 192.168.10.1 255.255.255.0` - you cannot use `.0` as an address here as it is too wide of a range. Look for green text to determine IP address and use the modifier next to the VLAN name (`.1`) - *NOTE* this becomes the gateway for any client attached to this VLAN
- `no shut`
- configure other ports by repeating the above steps

### Device Config
- Enter device
- IP Configuration
- Static
- Set IPv4 address to the first three octets of the VLAN address using the address specified on the device in the diagram as the last octet (`.2` for VLAN10 in the diagram) - eg `192.168.10.2` for VLAN10 in the diagram
- Subnet mask should be configured automatically - should match mask address specified on the router
- Set default gateway to the first three octets of the VLAN address - use the router port address as the last octet (`192.168.10.1` for VLAN 10 in the above diagram)

## Single Interface Inter-VLAN Routing ("Router on a Stick")
- Equipment:
    - Routers: 2621XM, 1841 or 2811 series
    - Switches: 2960 or 2950 series
    - PCs
- Cable up network from diagram - take care to ensure cables are in correct ports. Best to do PCs -> Switch first and then Switch -> Router so VLAN ports do not have their ports overtaken
- Divides one single physical router port into sub-interfaces - each sub-interface belongs to a distinct VLAN. Less ports used = cheaper.

Example:

![alt text](router-on-a-stick.png)

### Switch Config
- `enable`
- `configure terminal`
- `hostname <name>` - eg `hostname s1`
- `vlan <number>` - eg `vlan 10`
- `name <name` - eg `name Sales`
- configure other VLANs if required
- `interface range port-port` - eg `interface range fa0/1-12`
- `switchport mode access`
- `switchport access vlan <number>` - eg `switchport access vlan 10`
- `no shut`
- `exit`
- Configure trunk port
    - `interface port` - in this example the trunk port to the router is `interface fa0/1`
    - `switchport trunk encapsulation dot1q` - only required for layer 3 switches
    - `switchport mode trunk`
    - `exit`
- Check VLANs are configured correctly (`do show vlan`/`show vlan`)

### Router Config
- `enable`
- `configure terminal`
- `hostname <name>` - eg `hostname r1`
- Configure sub-interfaces
- `interface port/.vlan_number` - in this example the first sub-interface we configure is `interface fa0/0.10`
- `encapsulation dot1q vlan_number` - `encapsulation dot1q 10`
- `ip address <ip address> <mask address>` - one address per sub-interface which are defined in green on the diagram - in the above example `192.168.10`, `192.168.20`, `192.168.30` - the last octet is set to `.1` and will be the gateway address for any PC attached to the VLAN. In the above diagram the full addresses are `192.168.10.1`, `192.168.20.1`, and `192.168.30.1`
- `exit`
- Repeat above steps for each sub-interface that needs to be defined
- Configure the receiving end of the trunk port
- `interface port` - ex `interface fa0/0`
- `no shutdown`
- `exit`

### Device Config
- Enter device
- IP Configuration
- Static
- Set IPv4 address to the first three octets of the VLAN address using the address specified on the device in the diagram as the last octet (`.2` for VLAN10 in the diagram) - eg `192.168.10.2` for VLAN10 in the diagram
- Subnet mask should be configured automatically - should match mask address specified on the router
- Set default gateway to the first three octets of the VLAN address - use the router port address as the last octet (`192.168.10.1` for VLAN 10 in the above diagram)

## Elegant Inter-VLAN Routing - Layer 3 Switch
- Equipment:
    - Routers: 2621XM, 1841 or 2811 series
    - Switches: 3560 Layer 3 Switch
    - PCs

Example:

![alt text](layer-3-switch.png)

### Router Config
- `enable`
- `configure terminal`
- `hostname <name>` - eg `hostname r1`
- Enable EIGRP
    - `router eigrp 1`
    - Configure physical network connections
    - `network <address>` - in the diagram we only have one connection (`192.168.10.0`) for R1
    - Configure ports
        - `interface <port>` - eg `interface fa0/0`
        - `no shut`
        - `exit`
        - Repeat for all other ports (if required)
- Repeat for other routers (R2 in the above diagram would use `network 192.168.20.0`)

### Switch Config
- `enable`
- `configure terminal`
- `hostname <name>` - eg `hostname l3sw1`
- Configure VLANs - in the above diagram there is 4 - `10`, `20`, `30` and `40`
    - `vlan <number>` - eg `vlan 10`
    - `name <name>` - eg `name Sales`
    - configure other VLANs if required
    - `interface range port-port` - eg `interface range fa0/1-12`
    - `switchport mode access`
    - `switchport access vlan <number>` - eg `switchport access vlan 10`
    - `no shut`
    - `exit`
- No trunk ports required
- RIP needs configuring for R1 and R2
- `ip routing` - enable routing functions of L3 Switch
- Assign the four VLANs IP addresses (use `.1` modifier to prevent IP clashing)
    - `interface VLAN <number>` - eg `interface VLAN 10` message may appear saying state is up
    - `ip address <address> <mask>` - eg for VLAN 10 this would be `ip address 192.168.10.1 255.255.255.0`
    - Repeat above steps for each VLAN
- Enable EIGRP
    - `router eigrp 1`
    - `network <address>` - repeat for each VLAN address (use `.0` for the last octet of each address) - eg `192.168.10.0`
    - `exit`

### Device Config
- Enter device
- IP Configuration
- Static
- Set IPv4 address to the first three octets of the VLAN address using the address specified on the device in the diagram as the last octet (`.2` for VLAN30 in the diagram) - eg `192.168.30.2` for VLAN30 in the diagram
- Subnet mask should be configured automatically - should match mask address specified on the router
- Set default gateway to the first three octets of the VLAN address - use the router port address as the last octet (`192.168.30.2` for VLAN 30 in the above diagram)
- Repeat for other devices


# EtherChannel (PAgP + LACP)
- Equipment:
    - Switches: 2960
    - PCs

![alt text](etherchannel-setup.png)

Configuration
- Create switches and cable up PCs - DO NOT ADD CROSSOVER CABLES BETWEEN SWITCHES
- Use configuration defined for each PC for setting IP config
- Make switches pingable as no routers on the network:
    - `interface VLAN 1`
    - `ip address <address> <mask>` - for S1 this is `ip address 10.0.0.5 255.255.255.0`. You can find the IP used by using the same IP as the connected PC and take the last digit (`5` for S1)
    - `no shut`
    - `exit`
    - `ip default-gateway <ip address>` - eg `ip default-gateway 10.0.0.254` this is the DG specified on the PCs within the diagram above
    - Repeat for all switches within the network
- Set up EtherChannel trunks:
    - LACP (Passive)
        - `interface range <port-range>` - for diagram above when configuring S1 this would be `fa0/19-20` as defined by the crossover cables, ports 19 and 20 are defined as using LACP Passive
        - `channel-group 3 mode passive`
        - `exit`
        - Use EtherChannel as trunk:
            - `interface port-channel 3`
            - `switchport mode trunk`
            - `exit`
    - LACP
        - `interface range <port-range>` - for diagram above when configuring S1 this would be `fa0/21-22`
        - `channel-group 1 mode active`
        - `exit`
        - Use EtherChannel as trunk:
            - `interface port-channel 1`
            - `switchport mode trunk`
            - `exit`
    - PAgP
        - `interface range <port-range>` - for diagram above when configuring S2 this would be `fa0/23-24`
        - `channel-group 2 mode desirable`
        - `exit`
        - Use EtherChannel as trunk:
            - `interface port-channel 2`
            - `switchport mode trunk`
            - `exit`
    - These commands need to be ran on both sides of the connection
    - An error may be encountered when setting these up where it says the method is not enabled on the port, this is fine and resolves itself if/when the switches are connected.
    - Ports may take quite a while to stabilise so give them chance to turn green, they may go up and down between green and red
    - Some may also appear as red but will work fine, test connectivity to ensure network works as expected

# Loopback Interfaces
- Equipment:
    - Switches: 2960 or 2950 series

Configuration:
- `interface <port>` - eg `interface Loopback0` or `interface lo0`
- `ip address <address> <mask>` - eg `ip address 9.9.9.9 255.255.255.255`
- `no shutdown`
- `exit`
- Verify with `show ip interface brief` (or `do show ip interface brief`)

# EIGRP
## Basic
Configure EIGRP on Routers:
- Work through each router individually
- Use the IP address and subnet mask assigned to each interface to determine its network address
    - Examples:
        - `192.168.10.1 255.255.255.0` → `192.168.10.0`
        - `192.168.20.1 255.255.255.0` → `192.168.20.0`
        - `10.0.0.1 255.255.255.252` → `10.0.0.0`
- Example R1 from diagram above
    - `Fa0/0` → `192.168.10.1/24` → `192.168.10.0`
    - `Fa0/1` → `10.0.0.1/30` → `10.0.0.0`
    - `Fa1/0` → `192.168.50.1/24` → `192.168.50.0`
- `enable`
- `configure terminal`
- `router eigrp 1`
- Add each required network:
    - `network <ip address>` - eg `network 192.168.10.0`
    - Repeat for each network that should participate in EIGRP
- `exit`

## Wildcard Masks
- `network <network address> <wildcard mask>` - eg `network 192.168.10.0 0.0.0.255`
- Wildcard masks are the inverse of the subnet mask:
    - `/24` (`255.255.255.0`) becomes `0.0.0.255`
    - `/30` (`255.255.255.252`) becomes `0.0.0.3`

## Debugging EIGRP
- Check configured EIGRP networks
    - `show running-config`
    - `show ip protocols`
- Check interfaces + IP addresses
    - `show ip interface brief`
- Check EIGRP neighbors
    - `show ip eigrp neighbors`
- Check routes learned via EIGRP
    - `show ip route`
    - Appear with D in the route table
- Check topology
    - `show ip eigrp topology`

# HSRP
- Equipment:
    - Routers: 2621XM, 1841 or 2811 series
    - Switches: 2960 or 2950 series
    - PCs
- Cable up network from diagram, always do crossover cables for LACP/PAgP after all config has been done on the switches to prevent switch death due to bad config

![alt text](etherchannel-with-hsrp.png)

- Configure EIGRP on each Router that needs to participate

Configure Secondary Router:
- `enable`
- `configure terminal`
- `interface <port>` - this is the port that is connected to the switch which is running LACP/PAgP - in the above diagram this is `g0/0`
- `standby 1 ip <ip address>` - this will be the HSRP virtual address (also the same as the default gateway) defined in the diagram above - this is `192.168.1.1`
- Warning may be displayed for address, ignore this and wait for Speak/Standby/Active notification for the port

Configure Primary Router:
- `enable`
- `configure terminal`
- `interface <port>` - this is the port that is connected to the switch which is running LACP/PAgP - in the above diagram this is `g0/0`
- `standby 1 ip <ip address>` - this will be the HSRP virtual address (also the same as the default gateway) defined in the diagram above - this is `192.168.1.1`
- `standby 1 priority <priority>` - default priority is 100, so we set this to `150` to override the secondary
- `standby 1 preempt`
- Warning may be displayed for address, ignore this and continue on. Notification of port changing should pop up after the `standby 1 preempt` command is ran