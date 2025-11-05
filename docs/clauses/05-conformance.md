# Clause 5: Conformance

A claim of conformance to this standard is a claim that the behavior of an implementation of a MAC Security Entity (SecY) meets the requirements of this standard as they apply to MACsec protocol operation, management, and service provision as observed through external behavior. A claim may represent full conformance or conformance with Cipher Suite variance as defined in 5.4.

Conformance does not guarantee the overall security of the system containing the SecY, nor does it ensure that supporting protocols (e.g., key management or network management) are free from exploitable weaknesses.

Conformance does not constrain the type of system that may host a SecY beyond the capabilities defined in 5.3 and 5.4. Clause 11 illustrates SecY usage across several system types, including those specified in IEEE Std 802.1Q and IEEE Std 802.1X. Clause 15 defines Ethernet Data Encryption devices (EDEs) that reuse IEEE Std 802.1Q components to secure MAC Services transparently; additional EDE-specific conformance claims are described in 5.5 through 5.9.

## 5.1 Requirements Terminology

To align with IEEE and IEEE 802.1 usage, requirement levels follow this terminology:

- **shall** — mandatory requirement
- **may** — permitted implementation or administrative choice ("may" and "may not" have the same force)
- **should** — recommended choice; alternative behavior is permissible but less desirable

The PICS proforma in Annex A captures every normative occurrence of *shall*, *may*, and *should*.

The standard minimizes repetition by using *is*, *is not*, *are*, and *are not* for definitions and logical consequences of compliant behavior. Behavior that is permissible yet optional or controlled elsewhere is described with *can*; behavior absent from a conformant implementation is described with *cannot*.

## 5.2 Protocol Implementation Conformance Statement (PICS)

A SecY supplier claiming conformance shall complete the PICS proforma in Annex A and supply identifying information for both the supplier and the implementation.

An EDE supplier claiming conformance shall complete the PICS proforma in Annex D with identifying information and, as applicable, provide copies of the following PICS proformas, noting any exceptions required for conformance:

- For all EDE types: the Annex A SecY PICS proforma in this standard
- For all EDE types: the IEEE Std 802.1X PICS proforma
- For an EDE-M: the IEEE Std 802.1Q PICS proforma for a VLAN-unaware MAC Bridge
- For an EDE-CS: the IEEE Std 802.1Q PICS proforma for a Provider Edge Bridge
- For an EDE-CC: the IEEE Std 802.1Q PICS proforma for each C-VLAN component
- For an EDE-SS: the IEEE Std 802.1Q PICS proforma for each S-VLAN component

## 5.3 MAC Security Entity Requirements

An implementation of a SecY claiming conformance shall:

- Support Controlled and Uncontrolled Ports and use a Common Port as specified in Clause 10
- Support the MAC status and point-to-point parameters for the Controlled and Uncontrolled Ports as specified in 6.4, 6.5, and 10.7
- Process transmit requests from the Controlled Port per Secure Frame Generation (10.5)
- Process receive indications from the Common Port per Secure Frame Verification (10.6) before raising receive indications at the Controlled Port
- Encode and decode MACsec Protocol Data Units (MPDUs) as specified in Clause 9
- Use a 48-bit MAC address and 16-bit Port Identifier unique within that address assignment to identify each transmit SCI, as specified in 8.2.1
- Meet the performance requirements in Table 10-3 and 8.2.2
- Support the Layer Management Interface (LMI) operations required by the Key Agreement Entity (Clause 10)
- Provide the management functionality specified in 10.7
- Protect and validate MPDUs using Cipher Suites as specified in 14.1
- Support integrity protection with the Default Cipher Suite (Clause 14)

For each implemented Cipher Suite, a conformant SecY shall support at minimum:

1. One receive Secure Channel (SC)
2. Two receive Secure Association Keys (SAKs)
3. One transmit SC
4. Use of one of the two receive SAKs at a time for transmission, with the ability to switch within the time specified in Table 10-3

For each implemented Cipher Suite, a conformant SecY shall specify:

1. The maximum number of receive SCs supported
2. The maximum number of receive SAKs supported
3. The maximum number of transmit SCs supported

A SecY claiming conformance shall not:

- Introduce an undetected frame error rate exceeding that achievable when preserving the original frame check sequence, as required by 10.4
- Implement Cipher Suites beyond those in Clause 14 unless they meet the criteria in 14.2, 14.3, and 14.4.1
- Expose MACsec parameters via any SNMP version earlier than v3

A SecY claiming full conformance shall not implement Cipher Suites other than those specified in Clause 14.

> **NOTE** — Conformance with Cipher Suite variance is permitted as described in 5.4 and 14.4.1.

## 5.4 MAC Security Entity Options

A SecY claiming conformance may:

- Provide SNMPv3 access to MACsec parameters using the Clause 13 MIB module
- Support more than one receive SC
- Support more than two receive SAKs
- Support more than one transmit SC
- Support confidentiality protection with the Default Cipher Suite with or without a confidentiality offset (Clause 14)
- Implement additional Cipher Suites from Clause 14

If a SecY supports more than one transmit SC, it shall support a Traffic Class Table and an Access Priority Table per 10.7.17.

A SecY claiming conformance with Cipher Suite variance may use Cipher Suites not listed in Clause 14 provided they satisfy 14.2, 14.3, and 14.4.1.

## 5.5 EDE Conformance

EDEs combine bridging systems and components as defined in this standard. Clause 15 classifies EDE types (EDE-M, EDE-CC, EDE-CS, EDE-SS) and specifies their intended use.

The bridging systems and components comprising a conformant EDE shall comply with IEEE Std 802.1Q, IEEE Std 802.1X, and the SecY provisions of this standard, subject to the restrictions and clarifications defined for that EDE type.

Every conformant EDE shall:

- Provide exactly two externally accessible Bridge Ports: a red-side port and a black-side port
- Associate, for each required SecY, a Port Access Entity (PAE) that includes a KaY capable of operating MKA (15.2, 15.4, 15.5, 15.6, 15.7)

> **NOTE** — Red-side ports may be referred to as customer or edge ports; black-side ports may be referred to as provider or network ports. Usage in this standard aligns with multi-component bridge terminology and does not imply ownership.

## 5.6 EDE-M Conformance

An EDE-M implementation (15.2, 15.4) claiming conformance shall:

- Comprise a VLAN-unaware MAC Bridge as specified in IEEE Std 802.1Q-2018 clause 5.14, subject to this standard's constraints
- Incorporate a SecY in the black-side port interface stack (15.2, 15.4)
- Support securing connectivity within a customer or provider network (15.2) using the Nearest non-TPMR group address for group-addressed EAPOL PDUs and filter frames whose destination MAC address is a TPMR component Reserved Address or the Nearest non-TPMR Bridge group address
- Support securing connectivity across a Provider Backbone Network (PBN) to a peer EDE-M, MACsec-capable Customer Bridge, or EDE-CS (15.4, 15.5) using the Nearest Customer Bridge group address for group-addressed EAPOL PDUs and filter frames whose destination MAC address is a C-VLAN component Reserved Address except the Nearest Customer Bridge group address

An EDE-M may:

- Be configured to secure connectivity across a PBN to a peer EDE-CC (15.6) using the EDE-CC PAE group address for group-addressed EAPOL PDUs and filter frames whose destination MAC address is a C-VLAN component Reserved Address or the EDE-CC PAE group address
- Recover signaled priority from a C-VLAN tag and optionally priority-tag frames transmitted by the black-side port, as specified in 15.4

## 5.7 EDE-CS Conformance

An EDE-CS implementation (15.5) claiming conformance shall:

- Comprise a Provider Edge Bridge per IEEE Std 802.1Q-2018 clause 5.10.2, using exactly one C-VLAN component (the edge component) and one S-VLAN component (the network component)
- Incorporate a SecY in each internal Provider Edge Port interface stack (15.5)
- Support using the Nearest Customer Bridge group address for group-addressed EAPOL PDUs (15.5)

## 5.8 EDE-CC Conformance

An EDE-CC implementation (15.6) claiming conformance shall:

- Comprise two C-VLAN components as specified in IEEE Std 802.1Q-2018 clause 5.5—an edge component and a network component—internally connected per 15.6
- Incorporate a SecY in each internal Provider Edge Port interface stack (15.6)
- Support using the EDE-CC PAE group address as the destination MAC address for group-addressed EAPOL PDUs (15.6)
- Filter and block forwarding of frames whose destination MAC address is either a C-VLAN component Reserved Address or the EDE-CC PAE group address
- Preserve tagging for frames relayed from the red-side customer port to the black-side network port: transmit untagged frames untagged and C-tagged frames with the same C-VID (15.6)

## 5.9 EDE-SS Conformance

An EDE-SS implementation (15.7) claiming conformance shall:

- Comprise two S-VLAN components per IEEE Std 802.1Q-2018 clause 5.6—an edge component and a network component—internally connected per 15.7
- Incorporate a SecY in each internal Provider Edge Port interface stack (15.7)
- Support using the EDE-SS PAE group address as the destination MAC address for group-addressed EAPOL PDUs (15.7)
- Filter and block forwarding of frames whose destination MAC address is either an S-VLAN component Reserved Address (Table 8-2 of IEEE Std 802.1Q-2018) or the EDE-SS PAE group address
- Preserve tagging for frames relayed from the red-side customer port to the black-side network port: transmit untagged frames untagged and S-tagged frames with the same S-VID (15.7)

