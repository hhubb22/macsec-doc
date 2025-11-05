# Clause 1: Overview

## 1.1 Introduction

IEEE 802® Local Area Networks (LANs) are often deployed in networks that support mission-critical applications. These include corporate networks of considerable extent and public networks that support many customers with different economic interests. The protocols that configure, manage, and regulate access to these networks typically run over the networks themselves. Preventing disruption and data loss arising from transmission and reception by unauthorized parties is highly desirable, since it is not practical to secure the entire network against physical access by determined attackers.

MAC Security (MACsec), as defined by this standard, allows authorized systems that attach to and interconnect LANs in a network to maintain confidentiality of transmitted data and to take measures against frames transmitted or modified by unauthorized devices.

### MACsec facilitates

- Maintenance of correct network connectivity and services
- Isolation of denial-of-service attacks
- Localization of any source of network communication to the LAN of origin
- Construction of public networks that offer service to unrelated or mutually suspicious customers using shared LAN infrastructures
- Secure communication between organizations using a LAN for transmission
- Incremental and non-disruptive deployment that protects the most vulnerable network components

To deliver these benefits, MACsec has to be used in conjunction with appropriate policies for higher-level protocol operation in networked systems, an authentication and authorization framework, and network management. IEEE Std 802.1X™ provides authentication and cryptographic key distribution.[^clause1-mka]

MACsec protects communication between trusted components of the network infrastructure, thus protecting the network operation. MACsec cannot protect against attacks facilitated by the trusted components themselves and is complementary to, rather than a replacement for, end-to-end application-to-application security protocols. The latter can secure application data independent of network operation but cannot necessarily defend the operation of network components or prevent attacks using unauthorized communication from reaching the systems that operate the applications.

## 1.2 Scope

The scope of this standard is to specify provision of connectionless user data confidentiality, frame data integrity, and data origin authenticity by media-access-independent protocols and entities that operate transparently to MAC Clients.

> **NOTE** — The MAC Clients are as specified in IEEE Std 802®, IEEE Std 802.1Q™, and IEEE Std 802.1X.[^clause1-mac-clients]

### To this end, the standard:

- Specifies the requirements to be satisfied by equipment claiming conformance to this standard
- Specifies the requirements for MACsec in terms of provision of the MAC Service and the preservation of the semantics and parameters of service requests and indications
- Describes the threats, both intentional and accidental, to correct provision of the service
- Specifies security services that prevent, or restrict, the effect of attacks that exploit these threats
- Examines the potential impact of both the threats and the use of MACsec on the Quality of Service (QoS), specifying constraints on the design and operation of MAC Security entities and protocols
- Models support of the secure MAC Service in terms of the operation of media-access-control–method-independent MAC Security Entities (SecYs) within the MAC Sublayer
- Specifies the format of the MACsec Protocol Data Units (MPDUs) used to provide secure service
- Identifies the functions to be performed by each SecY and provides an architectural model of its internal operation in terms of processes and entities that provide those functions
- Specifies each SecY's use of an associated and collocated Port Access Entity (PAE, IEEE Std 802.1X) to discover and authenticate MACsec protocol peers and its use of that PAE's Key Agreement Entity (KaY) to agree and update cryptographic keys
- Specifies performance requirements and recommends default values and applicable ranges for the operational parameters of a SecY
- Specifies how SecYs are incorporated within the architecture of end stations, bridges, and two-port Ethernet Data Encryption devices (EDEs)
- Establishes the requirements for management of MAC Security, identifying the managed objects and defining the management operations for SecYs
- Specifies the Management Information Base (MIB) module for managing the operation of MAC Security in TCP/IP networks
- Specifies requirements, criteria, and choices of Cipher Suites for use with this standard

[^clause1-mka]: IEEE Std 802.1X™ defines Port-Based Network Access Control, including the MACsec Key Agreement protocol that distributes keys for this clause.
[^clause1-mac-clients]: IEEE Std 802®, IEEE Std 802.1Q™, and IEEE Std 802.1X™ specify the MAC Service clients relevant to MACsec deployments.
