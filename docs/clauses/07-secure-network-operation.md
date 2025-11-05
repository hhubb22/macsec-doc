# Clause 7: Principles of Secure Network Operation

This clause outlines the relationships and responsibilities that underpin secure network operation. It explains how the secure MAC Service (Clause 6) is established and consumed, and sets the context for the MAC Security protocol (Clause 8) and individual SecY behavior (Clause 10).

Secure network operation combines:

- Use of the secure MAC Service on every individual LAN in the network
- Application of appropriate security policies by MAC Service clients in end stations and forwarding devices
- Coordination among Key Agreement Entities (KaYs), MAC Security Entities (SecYs), and secure MAC Service clients

The terminology adopted here intentionally differs from other standards (e.g., IPsec's Security Parameter Index) to avoid ambiguity. Terms are defined normatively within this standard.

> **NOTE 1** — "Secure network" refers to the collective system of end stations, LANs, bridges, routers, and supporting services that rely on the secure MAC Service. The scope is limited to the contribution MACsec makes to overall security (Clause 1).
>
> **NOTE 2** — This clause summarizes concepts fully defined elsewhere; it adds no new normative provisions. Conformance criteria appear in Clause 5.
>
> **NOTE 3** — "Individual LAN" explicitly denotes a media-specific instance providing the MAC Service directly. Aggregations constructed by bridging or link aggregation are not individual LANs.
>
> **NOTE 4** — Examples and practices in this clause illustrate typical deployments; MACsec is not limited to the described scenarios. Any constraints necessary for correct operation of other protocols are stated explicitly.

## 7.1 Support of the Secure MAC Service by an Individual LAN

Each port that participates in the secure MAC Service includes both a KaY and a SecY. The KaY discovers peer KaYs, authenticates and authorizes them, and establishes the security relationships that the SecYs use to exchange frames:

- A secure Connectivity Association (CA) ensures the connectivity expectations described in 6.2 and 6.7.
- Each CA is realized through unidirectional Secure Channels (SCs) that use symmetric cryptography for transmission from one system to the rest of the CA.
- Each SC is maintained through an overlapping sequence of Security Associations (SAs).
- Every SA uses a fresh Secure Association Key (SAK) to deliver the guarantees in 6.8 and the services in 6.9 for its frame sequence.

> **NOTE** — An SC can remain active for years. Successive SAs with fresh SAKs ensure continued protection without interrupting service. Without an extended packet numbering Cipher Suite, a single SAK protects at most 2^32 − 1 frames; at minimum Ethernet frame sizes this limit is reached in roughly 17 seconds on a 100 Gb/s link, reinforcing the need for periodic key rotation.

### 7.1.1 Connectivity Association (CA)

A CA is the secure, symmetric, and transitive relationship among SecYs on a LAN. MACsec Key Agreement (MKA, IEEE Std 802.1X) creates and maintains the CA by authenticating participants, verifying authorization, and negotiating the Cipher Suite. Context for CAs is illustrated in the following figures (diagrams to be added during image cleanup):

- **Figure 7-1** — Two stations connected by a point-to-point LAN (insecure baseline)
- **Figure 7-2** — The CA established after successful authentication and authorization
- **Figure 7-3** — Two SCs supporting the CA for bi-directional communication
- **Figure 7-4** — Four stations sharing a LAN with full but insecure connectivity
- **Figure 7-5** — A CA that includes stations A, B, and C but excludes D
- **Figure 7-6** — Three SCs (one per transmitting station) supporting the shared-media CA

Stations may use multiple transmit SCs—for example, one per priority—to enforce in-order delivery or to tighten replay windows without sacrificing throughput.

### 7.1.2 Secure Channel (SC)

Each SecY transmits frames of a given priority over a single SC. SCs are unidirectional, point-to-multipoint constructs that can span multiple SAs. Each SC is identified by a Secure Channel Identifier (SCI) combining a 48-bit MAC address with a 16-bit Port Identifier.

> **NOTE 1** — Systems may require more SCIs than globally unique MAC addresses (e.g., virtualized environments or layered SecYs in provider bridges). The Port Identifier distinguishes SCs in such scenarios.
>
> **NOTE 2** — EPON Optical Line Terminals can dedicate an SC to the Single Copy Broadcast service, represented by a reserved SCI (Clause 12).
>
> **NOTE 3** — Because SCs are point-to-multipoint, transmitters decide when to switch from one SA to the next; receivers adapt automatically.

MKA informs each SecY about the SCI to use for transmission and the SCIs it must accept. When a SecY uses multiple transmit SCs, each SCI is represented as a separate MKA participant so that key agreement treats them as independent SecYs within the same CA.

### 7.1.3 Secure Association (SA)

An SC progresses through a succession of SAs, each keyed by a distinct SAK. The Secure Association Identifier (SAI) concatenates the SCI with a two-bit Association Number (AN), enabling receivers to identify the correct SAK for decryption and authentication (see Figure 7-7 — diagram pending).

- MKA generates and distributes SAKs to all SecYs in the CA. The same SAK may be used across different SCs provided every initialization vector remains unique (the default Cipher Suite incorporates the SCI into the IV).
- Transmit SecYs decide when to advance to a successor SA after confirming that peers are ready. Receivers therefore maintain SAKs for at least two SAs per inbound SC and can switch without notice; a 0.5 s bound accommodates technologies that reorder by priority.
- Transmitters do not interleave SAs on a single SC.

If a SecY lacks a usable SA for transmission (e.g., all PNs exhausted), it must signal the condition via management so that corrective action can occur before service is disrupted.

## 7.2 Multiple Instances of the Secure MAC Service on a Single LAN

KaYs can create distinct CAs on the same LAN to offer isolated secure service instances. Service multiplexing (e.g., VLAN tags) need not be integrity-protected, because modified frames will fail verification at the recipient. Each secure instance preserves symmetric and transitive connectivity, so clients perceive separate LANs.

When SecYs migrate between service instances, MAC_Operational temporarily transitions to FALSE to alert clients while membership converges. For any pair of SecYs whose relationship changes, at least one side experiences the transition. Once all participants join the new CA, MAC_Operational returns to TRUE. This behavior is analogous to partitioning or merging LAN segments.

Shared-media deployments can use source addresses—or, when necessary, SCIs—to select the appropriate inbound SecY, allowing bridges to provide distinct secure point-to-point services over a common medium (see 11.8). IEEE Std 802.11 [B2] defines its own mechanisms for differentiating secure associations.

## 7.3 Use of the Secure MAC Service

The secure MAC Service guarantees that each indication originates from a CA member and preserves the transmitted parameters (Clause 6). Authorization determines which stations may join the CA, but the service itself does not convey authorization levels. SecYs therefore forward all validated indications unless instructed otherwise by Clause 8 or Clause 9 semantics; higher-layer clients apply their own policies to preserve overall security.

> **NOTE** — For example, Spanning Tree Protocol entities must receive BPDUs from all legitimate bridges on the LAN. Rejecting BPDUs at the SecY based on stricter authorization could induce loops. Instead, the STP entity can discard BPDUs from unauthorized peers at the protocol level.

Policies in use should reflect the intersection of capabilities permitted to CA members. Policies may be:

- Selected locally based on authorization information provided via the LMI (Clause 10)
- Provisioned centrally and delivered securely (e.g., downloaded configuration, secured management channel)

Trust can be transitive. If a bridge participates in the CA, other members must decide how much to rely on the bridge's forwarding policies and its choices of downstream peers.

### 7.3.1 Client Policies

Example client policies (non-normative) include:

- Limiting which protocol procedures peers may invoke
- Segregating communications among distinct peer groups
- Filtering or discarding received frames that violate policy
- Recording exceptional policy decisions for administrative review

> **Example** — A VLAN-aware bridge may map frames from a given port to a specific VLAN based on that port's authorization, thereby segregating communications as described above.

### 7.3.2 Use of the Secure MAC Service by Bridges

Each Bridge Port attaches to an individual LAN (Clause 11), ensuring that configuration protocols—such as spanning tree—operate over a physical topology independent of the active forwarding topology they compute.

> **NOTE 1** — Provider Bridged Networks (IEEE Std 802.1Q) appear as single LANs to Customer Bridges, preserving this requirement even though provider infrastructure may relay frames internally.

PAEs and KaYs discover peers using group-addressed frames. Bridges and EDEs (Clause 15) filter these addresses to confine secure MAC Service instances to appropriate LAN scopes. By default, bridges use the PAE group address defined in IEEE Std 802.1X (also the Nearest Non-TPMR Bridge address in IEEE Std 802.1Q), which restricts discovery to customer LANs.

> **NOTE 2** — Reserved group MAC addresses help align the physical topology observed by control protocols with the topology protected by MACsec.

Bridge forwarding policies that interact with the secure MAC Service can include:

- Static Filtering Database entries
- RSTP/MSTP `restrictedRole` settings
- Port VLAN Identifier (PVID) assignments
- VLAN Translation Table configuration
- VLAN membership decisions and ingress filtering enablement
- Identification of ports as Provider Edge Ports
- Port priority configuration
- Priority remapping tables

> **NOTE 3** — A Bridge Port is the bridge's attachment to an ISS instance, used by the MAC Relay Entity and related higher-layer entities (IEEE Std 802.1Q).
>
> **NOTE 4** — The `restrictedRole` parameter prevents unauthorized peers from influencing spanning tree topology while still letting them select among redundant services. Enable it when peer authorization is limited.

Where authorization is limited, bridges can further constrain ports—for example, discarding frames from unexpected source MAC addresses or leveraging administrative services to ensure permitted addresses remain unique within the network.

