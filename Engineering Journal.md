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

# Configure Router
- Connect via Cabling
- Open MobaXTerm
- Connect like Switch
- `enable`
- `configure terminal`
- `hostname R1 <or any other name>`
- Configure ports
    - `interface fax/x` (eg `fa0/0`) for 2811
    - `interface gx/x` for 2911
    - `interface gx/x/x` for 4331
    - `ip address <ip address> <ip address>`
    - Turn on the port `no shutdown`/`no shut`
    - `exit`