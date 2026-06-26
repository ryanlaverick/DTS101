# Classifying Networks by Type:
Wide Area Networks (WAN):
- Covers large geographical area - could be national, international or even off-world (ISS)
- Tends to be based off facilities provided by common carriers (BT)
- Tend to be slower and more expensive than other network types (not necessairily true these days)
- Can be more expensive
- Used to connect remote sites of large organisations (Bank Branches)
- Must be regarded as very insecure

Metropolitan Area Network
- Cover medium scale geographical area (region, city)
- Tends to be based upon facilities provided by common carriers like a WAN
- Usually the same speed as a WAN but some regions have invested in an enhanced regional architecture
- Used to connect parts of a region together (University Campuses)
- Can be a collection of LANs spread across a very large site (Nissan)

Local Area Network (LAN)
- Small geographical areas
- Owned and managed by the organisation using it
- High speed
- Multiple technologies (copper, optical, wireless)
- Low cost per connection point
- Schools, Colleges, Supermarkets, Shops, Offices etc

Personal Area Network (PAN)
- Covers very small geographical area (maximum 10 metre spread)
- Intendes to connect together personal devices (computers, phones, tablets)
- Often wireless in nature - Bluetooth was designed for this (which is very insecure)
- Expanding area of the network business

Vehicular Area Network (VAN)
- Computer network contained in a vehicle
- Trains, Cars, Buses
- Often implemented as wireless
- Growing category as many now found on Trains, Cars, Buses etc

Virtual Local Area Network (VLAN)
- Gets itself mixed in with the rest
- Virtualised switch - compartment on a switch network

Wireless Local Area Network (WLAN)
- Local area sized network made up of unrelated technologies

Virtual Private Network (VPN)
- Encryption system sitting ontop of another network type
- Not dedicated networks themselves - can't do anything without a "carrier" network
- Encrypt all desired traffic between end-points to prevent:
    - Data being eavesdropped or stolen
    - Data being altered and retransmitted
    - Keeping conversations private

<br />
<br />

# Network Models
Before network standards:
- Original network systems tended to be unique to the organisation that set them uo
- Electrical systems were different:
    - Connection media types
    - Communication protocols
    - Voltage levels/connectors
- Resulted in different network systems not being able to directly communicate
- Made things like the Internet impossible

Network models can also be known as REFERENCE MODELS

Introduction of models provides:
- Layered approach - issues are isolated and can be troubleshooted accordingly
- Acts as a foundation for all future network development
- Common guide for all developers of network hardware and software to follow - can also be seen as standards or protocols
- Makes troubleshooting eaiser as network models follow a standard approach

OSI (Open Systems Interconnection Model)
- Seven layered stack
- Application (S) -> Presentation (S) -> Session (S) -> Transport (I) -> Network (H) -> Data-Link (H) -> Physical (H). 
- S - Software, I - Interface, H - Hardware

TCP/IP Model
- Complete compatible and equivalent model to OSI just with less layers
- "Department of Defense model"
- TCP/IP only exists within Transport Interface layer, badly named
- Developed around the same time as OSI
- Application (S) -> Transport (I) -> Internet (H) -> Network Access (H)

## Terminology
- Protocols - Rules of Comunication
- Network Protocols - Rules defining how pieces of network equipment communicate
- Protocol Data Unit (PDU) - moves between layers of a network model, used by protocols, each layer has many protocols which produce PDUs, used to pass information to other layers - up or down the stack

## Concentrating o nthe OSI Model
- Layer 7: Application
    - Only layer the user has direct communication with
    - Protocols:
        - Telnet/SSH
        - HTTP
        - SMTP
        - FTP
        - POP
    - PDU: Raw Data
- Layer 6: Presentation
    - Responsible for:
        - Ensuring data is readable by end systems
        - Formatting data and data structures
        - Negotiates data transfer syntax for the Application Layer
    - Protocols:
        - JPEG
        - MPEG
        - ASCII
        - WAV
        - OGG
        - WMF
        - DOCX
        - PDF
    - PDU: Formatted Data
- Layer 5: Session
    - Responsible for:
        - Establishes, managing and terminating sessions between network applications
    - Protocols:
        - NetBIOS
        - NFS
        - SQL
    - Assigns Port Numbers, ex 80 for Web Traffic
    - PDU: Formatted Data with Port Numbers
- Layer 4: Transport
    - Responsible for:
        - Reliable end-to-end transmission of data (checksums)
        - Establishes, maintains and terminates virtual circuits
        - Fault detection and recovery, information, flow control
    - Protocols:
        - UDP (User Datagram Protocol) - Unreliable transmission, "fire-and-forget" - video, speech, audio 
        - TCP (Transmission Control Protocol) - Reliable transmission (checksums)
    - PDU: Segments
- Layer 3: Network
    - Responsible for:
        - Logical addressing across the network - IP addressing
        - Best effort path determination
        - Best effort delivery
    - Protocols:
        - IP (Internet Protocol)
        - IPX
        - Appletalk
    - PDU: Packets
- Layer 2: Data-Link
    - Responsible for:
        - Media Access Control (MAC)
        - Logical Link Control (LLC)
        - Physical network addressing (MAC addressing)
    - Protocols:
        - Frame-relay
        - ISDN
        - Ethernet
    - PDU: Frames (smallest transmissable unit on a network)
- Layer 1: Physical
    - Responsible for:
        - Voltage levels
        - Connector types
        - Cable construction
        - Signalling
    - Protocols:
        - RS232-C(EIA/TIA232-C) - serial cables
        - RJ45 (8-pin connector on ethernet cables) or DB9 (9-pin connector on ethernet cables) connectors
    - PDU: Binary stream of zeros and ones


