# Cables and Connectors
9-pin DB9 connector -- check if the shield pin is connected correctly.

# Network Troubleshooting
Console into each device
Pull the config:
    - `show run`
    - Spacebar through
    - Go to earliest command (eg hostname) - make sure to set unique hostnames
    - Highlight display
    - CTRL+C
    - Paste into Notepad
    - Remove any non-user commands (!-prefixed lines) or unused features
    - Put "useful" commands back in (`enable`, `configure terminal` etc)
    - Use duplex auto - `no shutdown` should be in their place
    - Launch Simulator
    - Match Model (use same hardware in simulator as you're using physically)
    - Double click
    - Enter command line
    - Wait for CLI to boot
    - Add hostname for example `R1` for a router
    - Run through basic setup
    - Paste config
    - Fix issue(s) and test
    - Copy config from simulator using same method as above
    - Paste onto live server


Physical Layer (Layer 1) will know where a frame stops and starts, but no more - even a jumbo frame (greater than 1518 bytes) that is technically too big to process.

Data-Link Layer (Layer 2) can be used for detecting errors, but cannot fix them directly.

Network Layer (Layer 3) is responsible for logically addressing ports. If data is being sent to the wrong place, this may be the cause.

Cables cannot bend past 90 degrees, check cable radius if necessary.

Ensure there is a good number of collision domains.

Rollover cable - used to talk directly to the device

Rollover sockets - green, use straightthrough cable to preserve rollover between PC and device

Indicator lights with no data transmission - crossover or straight through cables are the wrong way round.

Slim server uses patch panel connector - see the number on the server and correlate it to the number port on the patch panel.

`reload` to reboot a switch or router - @ signs mean switch in reboot message, # signs mean router. ? means that there is a corruption somewhere.

## Common Issues
- Unable to reach host destination
    - Check if ports have switched from odds -> evens
    - Change Network Adapter -> VM -> Settings -> Network Adapter -> Change `VMnet0` to `VMnet1` or vice versa
- Request Timed Out
    - Is Defender enabled on the target machine?

## Clearing out previous configuration on Routers & Switches:
- Enter privilege mode (ensure terminal starts with `#`)
- Enter `write erase`
- Message for erasing the nvram filesystem will remove all configuration files
- Confirm
- Look at log messages, should see `Erase of nvram: complete`
- CTRL+R will bring the line back to the point you were writing before the confirmation message, or hit enter to move to a new line
- This has restored the default configuration
- Command only necessary for switches: `del vlan.dat`. On a router this will do nothing.
- Confirm delete action
- Delete copy in the flash, confirm this as well
- If an error message is displayed saying the file is not on the flash, ignore (the file was not on flash to begin with)
- Reboot router or switch - command `reload`
- Confirm reload. If config is asked to be saved before reload, select "no"
- @ signs mean switch in reboot message, # signs mean router. ? means that there is a corruption somewhere

## Debugging OSI Model
`ping` - tests Network, Data-Link and Physical Layers and partially Transport Layer work as expected.

`ssh`/`telnet (Terminal Emulation)` - tests all layers of the OSI model work as expected.

...or Connect Web Client to a Web Server - tests all layers of the OSI model work as expected.

## Routing Protocols
- Single Router network - no Protocol required (nothing to share)
- Hops are jumps between routers/switches
    - Number of hops between a device and an end point is one less than the number of routers/switches in the chain

- Routing Tables are shared between Routers running the same protocol on the same Network
    - Some send entire table to peers
    - Some send updates (new/dropped entry)

- RIP (Routing Information Protocol) Network:
    - Updates to routing table send as broadcasts
    - Limited to 15 hops (may favour less hops even when connection is slower)

- OSPF (Open Shortest Path First) - v1 can "almost" talk to v2 but can cause strange errors
- EIGRP - used for Cisco-only networks (multi-vendor - OSPF)

### Admin Distance
- Static Route - 1
- Default Route - 1
- OSPF - 10
- EIGRP - 90

### Default Routes
- Only one should exist with an AD of 1
- Backup default routes can exist if the primary goes down
    - Route Poisoning increases the AD of each backup default route by 1
        - Default 1 - AD 1
            - Default 2 - AD 2 (Backup)
            - Default 3 - AD 3 (Backup)

- `show ip network brief` when connecting to a live Switch to display current ports, bindings etc

# Configure Network
## Configure Device
- Identify IP Address of device
- Compare last octet of each address to bit borrowing table to get Mask (.66 falls within /27 range, for example)

## Configure Router
- Connect via Cabling
- Open MobaXTerm
- Connect like Switch
- `enable`
- `configure terminal`
- `hostname R1 <or any other name>`
- Configure ports
    - Identify IP address of each port to be bound
    - Compare last octet of each address to bit borrowing table to get Mask (.66 falls within /27 range, for example)
    - `interface fax/x` (eg `fa0/0`) for 2811
    - `interface gx/x` for 2911
    - `interface gx/x/x` for 4331
    - `ip address <ip address> <mask>` - ex `interface fa0/0`, `ip address 192.168.1.137 255.255.255.252`
    - Turn on the port `no shutdown`/`no shut`
    - `exit`
- If on Packettracer and connection to a switch is RED - troubleshoot port:
    - Hover over arrow on each side of the connection to see what port has been configured closest to the device
    - If trying to connect to 0/1 it may automatically bind to 0/0
        - To fix this hold click on the end of the connection to change
        - Click the device
        - Select the port to plug into

## EIGRP - Allow sibling Switches to talk to eachother
- Connect to terminal
- `router eigrp 255`
- `network <ip>` - `network 192.168.1.0` example for HSRP Default Gateway of 192.168.1.1 - make sure to do this for each domain (`1.0.0.0` for routers running on `1.0.0.x`)

## DHCP
- `ip dhcp excluded-address <start IP>|[<end IP>]` - make sure to do this for each statically assigned IP address (including those of routers and switches)
- Always configure DHCP Server *before* connecting it to any other switches or routers
- `ip dhcp pool <poolname>` - ex `ip dhcp pool fred`
- `network <network ID/subnet ID> <mask>` - ex `network 10.0.0.0 255.255.255.0`
- `default-router <default gateway address>` - ex `default router 10.0.0.1`
- `exit`
- Test DHCP by connecting to a Client, going into IP Configuration and selecting DHCP

## OSPF
Wildcard masks - used when setting up OSPF, inversion of ordinary masks
255.255.255.0 becomes 0.0.0.255
255.255.255.128 becomes 0.0.0.127
255.255.255.192 becomes 0.0.0.63
Convert all 1's to 0's
Convert all 0's to 1's

Masks are always 0's or odd numbers. Even numbers are not valid and are caused by an incorrect calculation.

DR - Designated Router (primary)
BDR - Backup Designated Router (secondary)
Grunts - all others

DR/BDR are elected by routers in a network
Can be influenced by setting one of these values below so that the desired device becomes DR/BDR:
- Highest value router ID - specified in dot decimal notation like an IP address when OSPF is configured
- If no router IDs set, then highest IP address value on the particular router wins. Adding loopback interfaces with higher IP addresses on them can be used to set this up (using /32 mask - 255.255.255.255)
- Or the highest OSPF priority level set wins (255 - always win, 0 - never win)