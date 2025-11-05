# Clause 11: MAC Security in Systems

This clause explains how MACsec integrates with common IEEE 802 system architectures, highlighting interface stacks, bridge behaviors, link aggregation, LLDP interactions, provider networks, and multi-access considerations.

## 11.1 MAC Service Interface Stacks

- A LAN MAC (e.g., IEEE 802.3) can present the MAC Service directly to LLC clients (`Figure 11-1` placeholder).
- Media-independent functions—VLAN tagging (IEEE 802.1Q), MACsec (this standard), etc.—operate above the MAC via the ISS/EISS (`Figure 11-2`).
- A SecY consumes an ISS access point and presents ISS service on its Controlled and Uncontrolled Ports, enabling MACsec to coexist with other convergence sublayers.
- Interoperability requires compatible media-independent functions arranged in the same order within the interface stack across communicating systems.

## 11.2 MACsec in End Stations

- A SecY maps directly between the station’s MAC Service and ISS.
- Unwanted destination MAC addresses are discarded prior to SecY processing; source MAC addresses reflect the station.
- MACsec can be the only media-independent function in the stack (`Figure 11-3`).

## 11.3 MACsec in MAC Bridges

- IEEE 802.1Q MAC bridges forward frames between ISS access points on their ports.
- Each insecure LAN interface feeding a Bridge Port should host a SecY, exposing secure ISS instances to bridge functions (`Figure 11-4`, `Figure 11-5`).
- Aggregated links: each physical LAN within a Link Aggregation bundle owns its own SecY before aggregation (see 11.5).

## 11.4 MACsec in VLAN-aware Bridges

- VLAN-aware bridges (IEEE 802.1Q) add tagging/translation above the MAC.
- MACsec fits below VLAN functions, providing secure ISS instances that the VLAN-aware bridge consumes (`Figure 11-6`, `Figure 11-7`).
- Ensure consistent placement so MACsec-protected frames remain compatible with provider/service VLAN operations.

## 11.5 MACsec and Link Aggregation

- Each aggregated physical link requires an independent SecY.
- The Link Aggregation sublayer operates over the secure ISS delivered by each SecY, preserving aggregation behaviors while protecting individual links.
- Replay windows and SCI management should account for parallel links with potentially reordered delivery.

## 11.6 Link Layer Discovery Protocol (LLDP)

- LLDP frames ordinarily traverse the Uncontrolled Port.
- MACsec can secure LLDP exchanges by using appropriate policy: e.g., leave LLDP on the Uncontrolled Port for discovery, or deploy MACsec-aware LLDP extensions when confidentiality/integrity is required.
- Ensure LLDP group MAC addresses remain reachable when filtering via MACsec, particularly in provider environments.

## 11.7 MACsec in Provider Bridged Networks (PBNs)

- Provider bridges deliver ISS/EISS instances to customer equipment.
- Customer-edge SecYs protect traffic before entering the provider network; provider-edge SecYs can be deployed for inter-provider security.
- MACsec supports multiple SCs to isolate customer services; SCI management helps map customer traffic across provider infrastructure.
- Filtering of SecTAG-bearing frames at provider boundaries prevents leakage of customer MACsec frames into unintended domains.

## 11.8 MACsec and Multi-access LANs

- Multi-access LANs (e.g., shared media with multiple SecYs) demand careful SCI and KA provisioning.
- SecTAGs on Uncontrolled Port frames (E=1, C=0) allow association of insecure traffic with specific SCIs for future protocols.
- Frames on multi-access LANs may omit SecTAGs if only one bidirectional unicast exists per station pair; otherwise SCI-based mapping is required.
- Stations can instantiate multiple SecYs (“virtual ports”) on demand; a demultiplexing function delivers frames to the correct SecY based on SCI (`Figure 11-14`, `Figure 11-15`).
- Multi-access LANs should not interconnect multiple bridges due to dynamic connectivity shifts; they suit end-station attachment at network edges.

---

**Figure Placeholders** — Figures 11-1 through 11-15 need recreation or replacement with locally stored diagrams once available.

