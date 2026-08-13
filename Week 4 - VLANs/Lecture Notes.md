# VLANs
VLAN Trunks run off different protocols, which by default carry all VLANs. These are `dot1q`, `ISL` etc

Note: Higher models of switches may require trunking protocol definition to be done manually: etc `switchport trunk encapsulation dot1q` (3560, 3650 on Packet Tracer only)

## Inter VLAN Routing
- Requires a Layer 3 Switch
- Seen as the more elegant method
- Layer 3 Switch is a Layer 2 Switch with a Routing Engine in the same chassis
- More economical - less connections meaning it reduces cost and increases efficiency

