# Clause 15: Ethernet Data Encryption Devices (EDEs)

This clause describes how purpose-built Ethernet Data Encryption devices apply MACsec to secure customer/provider networks without requiring upgrades to existing bridge infrastructure.

## 15.1 Characteristics

- EDEs are two-port systems comprising red-side (customer) and black-side (provider) interfaces.
- Internally they include bridge components (edge/network) with embedded SecYs and associated KaYs.
- Support transparent connectivity while preserving VLAN tagging, service selection, and QoS.

## 15.2 EDE-M (Customer Bridges)

- Acts as a VLAN-unaware customer bridge with a SecY on the black-side port.
- Secures traffic within customer or provider networks and supports communication with peer EDE-Ms, MACsec bridges, or EDE-CS devices.
- Filters reserved addresses appropriately; leverages Nearest non-TPMR Bridge or Customer Bridge group addresses.

## 15.3 EDE-CS (Customer-Server)

- Combines a C-VLAN edge component with an S-VLAN network component.
- SecYs inserted on provider edge ports; uses Nearest Customer Bridge group address for EAPOL.

## 15.4 EDE-CC (Customer-Customer)

- Two C-VLAN components (edge/network) with SecYs on provider edge interfaces.
- Preserves untagged/C-tagged frames, filters EDE-CC reserved addresses, supports provider backbone traversal.

## 15.5 EDE-SS (Server-Server)

- Two S-VLAN components enabling S-tagged provider services.
- Similar filtering/forwarding rules to EDE-CC but with S-VLAN semantics.

## 15.6–15.8 Interoperability and PBN Integration

- Details how EDE variants connect across Provider Bridged Networks, preserve priority, and interwork with CFM/UNI access.
- Emphasizes group-address handling and replay protections when bridging across provider domains.

**Figure Placeholders** — Clause 15 contains numerous figures (15-1 to 15-9) illustrating topologies and interface stacks; recreate as needed.

