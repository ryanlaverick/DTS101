# Cables and Connectors
9-pin DB9 connector -- check if the shield pin is connected correctly.

# Network Troubleshooting
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