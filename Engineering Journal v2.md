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


# EtherChannel
