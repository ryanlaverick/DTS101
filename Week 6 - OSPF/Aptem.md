I spent this time learning about OSPF (standing for "Open Shortest Path First") - this is a routing protocol that runs for both IPv4 and IPv6 (depending on the version of OSPF that is being run - v1/v2 for IPv4, v3 for IPv6) hwich determines the "optimal" path for packets to be sent across a network.

OSPF operates on the pricinciple of finding the most "optimal" path between routers, based on cumulative bandwidth of all network devices. OSPF will favour high-speed connections that may require more "hops" between routers, over a path that has less hops but is more congested (lower bandwidth)

OSPF has an administrative distance of 110, meaning OSPF routes will be preferred over RIP (120) but may be overridden by EIGRP routing (90)

OSPF works by electing a router as the designated router which handles all traffic entering the network - typically this is the router on the network with the best resources available. OSPF will then elect a backup designated router which will automatically take in the case that the designated router goes offline (or is no longer discoverable) All other routers are known as "grunts" which do no processing of their own. If the designated router, and backup designated router are disconnected from the network OSPF will automatically elect two new routers to take their place.

OSPF elections can be influenced in a number of ways (such as setting router IDs, and OSPF priority levels) to ensure specific routers do, or do not become the DR or BDR. This is useful if there are routers on the network which would be incapable of handling the responsibility and throughput of traffic required to become the DR/BDR. 

We then used this knowledge to create a network which comprised of multiple sub-networks, each created by a different team within the class. These sub-networks had a range of requirements but needed to run OSPF for routing, and demonstrated how we could influence elections.

I have covered the following KSBs: K1, K2, K3, K5, K11, K16, K19, K61, K64, K65, K67, S9, S12, S57, S58, S59, S61, S62, B1, B2, B3, B4, B6, B7, B8