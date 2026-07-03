Backbone ports on a Switch - used for linking switches together via separate interface (not taking up a user port)

Switches make broadcast domains bigger

## Routing
- Method by which we get data from end of a network to another
- Routing is the process where the best pat between two desired end points is worked out by devices called routers
- Can be like a roundabout in a road system
    - Traffic enters on one interface (or "road")
    - Depending on the destination IP in the packet header it is forwarded out of another interface
    - If destination is not recognised and no default route exists, traffic is dropped

## Routing Tables
Each Router builds up a Routing Table which:
- Contains directions to live networks
    - Table contains a "time" for each address, after no response for x time from network the entry is killed/dropped
    - If a link goes down then its associated route is dropped
- Manages broadcast domains
- Packets entering router:
    - Each one has a logical destination address - compared with routing table entries
    - If there is a match - packet is forwarded out of appropriate interface
    - If not - it is dropped


## Sharing Routing Information
- Not all networks and subnets connected to all routers in a network - very expensive
- Routers running the same routing protocol on a single network will share a routing table
    - Even the furthest router knows where/what direction to send packets
- Often called dynamic routing protocols - learn as they go
- Packets can flow through Router X, and Router Y will automatically know about it (if they share protocol and network)
- Some build up entire network topologies in memory:
    - Incredibly efficient
    - Requires lots of memory (not suitable for home routers)
- Some will send entire routing trees in dynamic routing, some will just send changes (address added, dropped)


## Routing Protocol Types
- Distance Vector (£)
    - Less CPU intensive
    - Work on metrics to decide what routes are best
    - Hop limits (1 hop = 1 inter-router jump)
    - Tend to send full/partial routing table updates to its directly connected peer routers
    - RIP (Routing Information Protocol):
        - Old protocol
        - Commonly used open standard on practically all router hardware platforms
        - Unable to recognise slow neighbours/nodes
        - Used for small networks - home
        - Limited to using hop count as its only metric - upper limit of 15 hops
        - May favour slow, smaller hops instead of high-speed high hops
        - All updates send as broadcasts - may cause broadcast storms
        - 3 common versions
            - 1: classful addressing only (does not understand subnets), uses periodic broadcasts of its entire routing table with neighbours
            - 2: IPv4, broadcasts sent as multicasts - no broadcasts, less traffic other devices must listen to
            - NG (Next Generation): IPv6
- Link State (£££)
    - Powerful equipment required
    - Keep picture of entire network layout in topology database
    - Suitable for larger networks
    - Scale well
    - Use "Hello" packets to neighbouring routers to ensure they are still alive
    - Send small updates (more bandwidth available for users)
    - Update across a link-state controlled network if a router loses contact, or senses a new neighbour router (packet has come from a new device/address)
    - All update and recalculate routing tables and topology database
    - Heavier load
    - Open Shortest Path First (OSPF):
        - OSPFv2 for IPv4 (difficult to configure)
        - OSPFv3 for IPv6
        - No hop limit
        - Breaks network down into more manageable network areas to aid traffic control
        - Open standard
        - Requires powerful hardware to run
        - Complete topology database
        - AD 10
- Hybrid Routing Protocol - EIGRP (Enhanced Interior Gateway Routing Protocol)
    - Reasonably powerful hardware required
    - Uses lots of metrics
    - Combines some of Distance Vector and some of Link State
    - Adopted "hello" packets from Link State
    - Hop count limit of 224 hops
    - Easy to configure & use
    - Proprietary (Cisco)
    - Versions for IPv4 and IPv6 exist
    - Conversion speed is incredibly quick
    - Sends only changes to routing table instead of the entire table
    - AD of 90

## Static & Default Routes
- Smaller AD (administrative distance) value, the more preferred the route in the routing table
- Static routes are used for networks that never move:
    - Manually added to routing table
    - Table update can be triggered manually
    - AD Distance of 1
    - Useful for default routes which can be used to redirect traffic that would have otherwise been dropped
    - Multiple can exist in the routing table
- Default route
    - Redirect for traffic that would have been dropped
    - "Don't Care" route
    - AD of 1
    - Usually 1 in a system (more means which route to take)
    - Backup Routes can exist:
        - Route Poisoning
        - Increases AD by 1
            - Default 1 - AD 1
            - Default 2 - AD 2 (Backup)
            - Default 3 - AD 3 (Backup)

