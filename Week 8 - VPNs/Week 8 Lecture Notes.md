# VPNs

## Tunnelling Protocols
- Encapsulates a set of packets inside a set of other packets from a different network conversation

## Generic Route Encapsulation (GRE)
- Form of tunnelling protocol
- Provides virtual point-to-point link between two remote end points
- Does not matter how far aprt these end-points are as long as they are pingable from either end
- Needs 2-host subnet assigned to it as part of the IP scheme for the network (ensure it is contained within the IP table) - first host address at one end, last host address at the other
- Subnet is then advertised on the two host routers within the routing protocol is used (RIP, OSPF, EIGRP)
- Packets themselves run from the physical address to physical address
- Payload packets run from "private" subnet virtual ip address to virtual ip address
- Has no inherent encryption, useful for keeping traffic separated but can participate in interior routing protocols
- Can connect across multiple networks, and can connect across areas of private networks too
- Not protected by encryption, though there is a method used to encrypt and protect the GRE payload using a VPN

### Router Configuration
- `enable`
- `configure terminal`
- `hostname <name>` - eg `hostname R1`
- `interface tunnel <number>` - eg `interface tunnel 0`
- `ip address <ip address> <mask>` - the virtual subnet IP which will be advertised on the interior routing protocol, eg `ip address 200.1.1.1 255.255.255.0`
- `tunnel source <port>` - this is the exit interface on the router where this side of the tunnel begins, eg `tunnel source fa0/0`
- `tunnel destination <ip address>` - this is the IP address of the physical (point to point) connection at the other end of the tunnel, eg `tunnel destination 190.1.1.2`

## Virtual Private Networks (VPNs)
- 5 generic stages in the lifecycle of a site-to-site VPN:
- Specifying interesting traffic
    - Where we define what network traffic is to be protected:
        - Traffic that should not be modified without detection
        - Should not be read by anyone in the middle
        - Only pre-determined end points should be able to decrypt packets/traffic
    - Uninteresting traffic should not be encrypted
    - Encryption happens in real-time so causes increased CPU load
    - ACLs are used t odefine interesting traffic
- Internet Key Exchange Phase 1 (IKE Phase 1)
    - Phase 1 tunnel is set up so that we can exchange keys securely
    - Keys are used to set up phase 2 tunnel which will actually transport data payload
    - Two end point devices exchange security proposals in the form of transform sets
        - List of security parameters for tunnel set-up
        - Transform sets at each end must match or they won't trust eachother (phase 1 tunnel won't form)
    - If transform sets match then the two end-points swap public encryption keys (known as the Diffie-Hellman encryption protocol) - used to exchange encryption keys for the next stage
    - Final conversation to perform authentication of the peers
        - Hashing function is used to confirm the identity and to ensure that no rogue devices can establish a secure connection
    - Transform Set Parameters
        - Both ends must match or the tunnel won't establish
        - IKE Encryption Algorithm (DES, 3DES, AES)
        - IKE Authentication Algorithm (MD5, SHA)
        - IKE Key (pre-shared or RSA signature)
        - Diffie-Hellman Version (Cisco IOS supports 1, 2 & 5)
        - IKE Tunnel Life Time (seconds, time before the tunnel has to be renegotiated)
    - Transform set is bundled up into what is knwon as an IKE Phase 1 Policy - makes use of ISAKMP to achieve its aim (Internet Security Association and Key Management Protocol)
- Internet Key Exchange Phase 2 (IKE Phase 2)
    - Consists of:
        - Negotiating IPSec security parameters using IPSec Transform Sets
        - Establishment of IPSec security associations
        - Periodic renegotiation of IPSec security associations to ensure security
        - Additional Diffie-Hellman key exchange as an optional feature
    - Transform Set Parameters
        - Both ends must match or the tunnel won't establish
        - IPSec Protocol - Authentication Header (AH) or Encapsulated Secure Payload (ESP)
        - IPSec Encryption Type (DES, 3DES, AWS)
        - IPSec Authentication (MD5, SHA)
        - IPSec Mode (Tunnel, Transport - now obsolete)
        - IPSec Security Association Lifetime (seconds, or kilobyte length)
    - Once a Phase 2 Tunnel is established the Phase 1 Tunnel can be torn down (until renegotiation is required) - Phase 1 is only used for establishing Phase 2 and serves no other purpose
- Secure Data Transfer
    - Once both tunnels established successfully, encrypted traffic should flow normally
    - Network Traffic Analyser (WireShark) can test if this is succeeding
- VPN Teardown
    - Occurs when time limit (or data limit is reached), or no interesting traffic flows then the VPN will be town down until another packet of interesting traffic needs to pass through the tunnel
    - Phase 1 and 2 procedures will then repeat and a tunnel will be set up again
