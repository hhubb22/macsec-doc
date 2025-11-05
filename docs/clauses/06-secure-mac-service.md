# Clause 6: Secure Provision of the MAC Service

MACsec provides secure communications between stations attached to the same LAN. Each station's MAC Security Entity (SecY) authenticates and authorizes its peers and uses the insecure MAC Service provided by the LAN to deliver a secure MAC Service to its client (see Figure 6-1; figure capture pending). The requirements in this clause aim to preserve the semantics of the MAC Service while introducing security protections.

> **NOTE 1** — This clause introduces concepts that are specified in greater detail elsewhere in the standard; it does not add new normative requirements beyond those clauses.
>
> **NOTE 2** — MACsec alone cannot secure a Bridged Local Area Network; overall security also depends on individual LAN security, client policies (7.3), and bridge integrity.
>
> **NOTE 3** — Authentication and authorization are outside the scope of this standard; MACsec assumes mutually authenticated and authorized service access points.
>
> **NOTE 4** — The MAC Service and secure MAC Service are provided at a service access point for a single client (LLC Entity or another entity offering MAC/ISS services per IEEE Std 802.1AC and IEEE Std 802.1Q).

## 6.1 MAC Service Primitives and Parameters

The MAC Service (IEEE Std 802.1AC) provides unconfirmed, connectionless data transfer: a request primitive at a source service access point (SAP) leads, with high probability, to an indication primitive at intended destination SAPs. Each request results in at most one indication per destination SAP.

Each request/indication primitive carries four parameters:

- Destination Address
- Source Address
- Priority
- MAC Service Data Unit (MSDU)

The MAC Service may be delivered by a single LAN or a Bridged Local Area Network. End-station LLC clients use the MAC Service defined in IEEE Std 802.1AC. MAC Bridges use the MAC Internal Sublayer Service (ISS), which exposes additional parameters (e.g., frame check sequence) necessary for relay operations. Unless otherwise noted, references to the MAC Service in this clause cover both the LLC-facing service and the ISS. Multiple MAC Service instances can be supported via the Enhanced Internal Sublayer Service (EISS) in VLAN-aware Bridges (IEEE Std 802.1Q-2018, 6.6); when VLAN tagging is used, EISS parameters are encoded within the ISS MSDU.

> **NOTE 1** — The MAC Service abstraction guides the development of client protocols across diverse MAC methods.
>
> **NOTE 2** — Some specifications refer to the priority parameter as *user priority*. ISS functions derive an *access priority* for local medium access based on the user priority.

A MAC Service instance supplied by a single LAN preserves ordering of requests and indications with identical priority values. Instances provided via the EISS preserve ordering for matching destination/source/priority tuples (individual addresses) or destination/priority pairs (group addresses).

> **NOTE 3** — Provider Bridged Networks can leverage EISS to deliver MAC Service instances that emulate single-LAN behavior (aside from ordering nuances), enabling standard MACsec operation across service provider infrastructure.

## 6.2 MAC Service Connectivity

Single LANs—point-to-point or shared media—provide symmetric and transitive connectivity among attached stations: a request from any station reaches all peers with high probability. Media access functions filter frames for LLC-facing SAPs to match subscribed addresses.

> **NOTE 1** — Symmetric connectivity means if station A can reach B, then B can reach A; transitive connectivity means if A reaches B and B reaches C, then A reaches C.
>
> **NOTE 2** — Some MAC devices suppress delivery of frames with unwanted destination addresses.

Protocols relying on full connectivity (e.g., OSPF, spanning tree) can degrade or fail when symmetry or transitivity is broken. VLANs are preferred to administratively filtering frames because each VLAN maintains full intra-VLAN connectivity.

> **NOTE 3** — Legacy STP may form loops without symmetric connectivity; RSTP detects such issues but may deny service until resolved. Protocols like OSPF rely on full peer reachability. Non-broadcast LANs that only deliver frames to explicit destinations can hinder MAC address learning and cause persistent flooding.

## 6.3 Point-to-Multipoint LANs

A point-to-multipoint LAN provides connectivity from a distinguished station (e.g., OLT, access point) to multiple other stations, and from each of those stations back to the distinguished station. Direct station-to-station connectivity exists only via the distinguished node. Efficient multicast/broadcast is provided by the distinguished station issuing a single request that yields indications at all subscribed receivers.

> **NOTE** — Examples include IEEE 802.3 EPON (Clause 12), IEEE Std 802.11™ [B2], and specific provider VLAN deployments. Terminology varies by technology (OLT/ONU, head-end/modem, etc.).

## 6.4 MAC Status Parameters

Each SAP can expose status parameters describing operational state and administrative controls:

- **MAC_Enabled** — TRUE if service use is permitted; otherwise FALSE. Set by administrative controls.
- **MAC_Operational** — TRUE when service requests and indications can occur.

IEEE Std 802.1AC and IEEE Std 802.1Q define how these parameters are determined for insecure ISS/EISS operation; this standard defines their determination for the secure MAC Service.

> **NOTE** — Accurate MAC_Operational reporting enables rapid protocol convergence for RSTP, MSTP (IEEE Std 802.1Q), and LACP (IEEE Std 802.1AX™ [B4]). Without it, connectivity loss is inferred only after repeated frame losses.

## 6.5 MAC Point-to-Point Parameters

SAPs can also expose point-to-point status parameters:

- **operPointToPointMAC** — TRUE if the service is treated as connecting to at most one peer; FALSE otherwise.
- **adminPointToPointMAC** — Administrative control with values:
  - *ForceTrue*: operPointToPointMAC must be TRUE
  - *ForceFalse*: operPointToPointMAC must be FALSE
  - *Auto*: operPointToPointMAC follows the service-providing entity's determination

IEEE Std 802.1AC and IEEE Std 802.1Q describe how these parameters are derived for insecure ISS/EISS; this standard defines the secure MAC Service behavior.

> **NOTE** — RSTP/MSTP rely on operPointToPointMAC during rapid reconfiguration. LACP uses the parameter to ensure only point-to-point links are aggregated.

## 6.6 Security Threats

Misconfiguration or abuse of MAC Service expectations (connectivity, parameter preservation, request/indication relationships, status signaling) can result in:

- Loss of ability to issue service requests
- Indiscriminate or targeted loss of indications
- Repetition of indications at intended destinations
- Modification of address or data parameters
- Insertion of additional indications with altered parameters
- Indications delivered to unintended recipients
- Delayed indications disrupting operation
- Disclosure of MSDUs to unauthorized parties

Abuses facilitate denial of service, theft of service, confidentiality breaches, and resource hijacking. Many exploits are straightforward for attackers with LAN access because the MAC Service does not authenticate source addresses or restrict eavesdropping. Techniques such as passive wiretapping, masquerading, and man-in-the-middle attacks (often via spoofed source addresses) are common precursors to:

- Denial of service (broad or targeted)
- Service theft
- Access to confidential data
- Modification of confidential data
- Unauthorized control of protected resources

MACsec does not mitigate brute-force denial-of-service attacks that exploit the underlying media access method or its control frames.

## 6.7 MACsec Connectivity

Key agreement protocols defined in IEEE Std 802.1X establish Secure Connectivity Associations (CAs) as fully interconnected subsets of ISS SAPs. MACsec operates within a single CA and reflects CA health through MAC Service status parameters:

- **MAC_Operational** is TRUE only if the CA is symmetric, transitive, and the local SecY can both transmit and receive.
- When stations with MAC_Operational TRUE join an existing CA, either all original members or all joining members temporarily transition MAC_Operational to FALSE so that clients observe the change; the KaY determines which set transitions and signals via the LMI.
- Stations whose MAC_Operational is FALSE when joining a CA do not affect existing members; their MAC_Operational transitions to TRUE after joining completes (typical single-station join).
- If **adminPointToPointMAC** is Auto and MAC_Operational is TRUE, **operPointToPointMAC** is TRUE only when at most one peer participates in the CA. If adminPointToPointMAC is ForceFalse, operPointToPointMAC remains FALSE regardless of CA size.

> **NOTE 1** — ISO/IEC 15802 defines Connectivity Associations without reference to particular MAC methods.
>
> **NOTE 2** — EPON specifics appear in Clause 12.
>
> **NOTE 3** — KaY communication does not depend on MACsec operation.

Connectivity abuse remains possible if the underlying MAC Service is compromised (e.g., frames delivered to unintended ports before SCI validation). MACsec's validation steps—SCI checking, replay protection, integrity verification—limit the effect of such abuse.

## 6.8 MACsec Guarantees

For SAPs within a CA, MACsec ensures each indication:

1. Results from a request issued by another CA member.
2. Preserves the request's parameter values.

MACsec can additionally ensure that:

3. No more than one indication arises from a single request.
4. Indications occur within a known bounded interval after the request.
5. MSDU octet values remain confidential to CA members.

MACsec does **not**:

- Hide the existence of requests, their address parameters, or MSDU lengths from non-members.
- Validate the correctness of request parameters at transmission.

The bounded interval in item 4 is typically longer than MAC Service transit-delay limits; higher-layer protocols often embed their own timing fields inside the MSDU, which MACsec protects.

## 6.9 Security Services

MACsec's guarantees enable the following services for participating stations:

- Connectionless data integrity (6.8 items 1–2)
- Data origin authenticity (6.8 item 1); in multipoint contexts the authenticated origin is any CA member
- Confidentiality (6.8 item 5)
- Replay protection (6.8 items 3–4)
- Bounded receive delay
- Mitigation of certain denial-of-service effects

MACsec does **not** provide non-repudiation (ISO/IEC 7498-2) nor does it defend against traffic analysis (ISO/IEC 7498-2). MAC Bridges should pair MACsec with policies that verify MAC-address/resource bindings to avoid unintentionally amplifying attacks when forwarding frames with erroneous source addresses.

## 6.10 Quality of Service Maintenance

QoS can degrade through direct attacks on MAC methods or indirect attacks on their resource allocation—for example, via masquerading or unauthorized data modification. MACsec does not secure MAC control frames (which remain local to a LAN and are not forwarded by bridges); abuses leveraging such frames must therefore be mitigated by other means.

> **NOTE** — Where control functions can operate over the ISS, establishing CAs before executing those functions allows MACsec to protect their parameters and to bound abuse. The OSI model does not require management and control to remain entirely within the same layer; for example, SNMP can manage MAC Bridges.

Security processing can affect QoS. MACsec addresses the following aspects:

- **Service availability** — Key agreement delays or authentication failures can reduce availability; robust policies mitigate this risk.
- **Frame loss** — MACsec adds minimal frame loss beyond that caused by larger frame sizes. Provider networks with expedited forwarding or link aggregation can trigger frame reordering; replay protection settings should accommodate expected reordering.
- **Frame misordering** — MACsec does not introduce new misordering mechanisms; separate Secure Channels per priority can minimize discard.
- **Frame duplication** — No retransmission mechanisms are added; each frame is independent.
- **Frame transit delay** — Additional delay stems from larger MSDUs and cipher processing. Implementations must adhere to the limits in Table 10-3, which are well below MAC Service bounds.
- **Frame lifetime** — Increased delay can extend frame lifetime modestly (typically bounded near two seconds).
- **Undetected frame error rate** — MACsec does not raise the error rate; FCS still covers the entire frame, and MACsec's integrity checks reduce undetected modifications. Techniques in IEEE Std 802.1Q-2018 Annex O limit error-rate increases when FCS coverage changes.
- **Maximum MSDU size** — Added security information reduces the maximum MSDU size available to clients.
- **Frame priority** — When securing ISS instances that support EISS, MACsec preserves encoded priority. Bridges accessing the ISS can observe or adjust priority even if confidentiality is enabled. Access Priority Tables (10.7.17) translate user priority to medium access priority.
- **Throughput** — Cipher Suites are selected for efficient implementation across LAN rates, minimizing throughput impact.
