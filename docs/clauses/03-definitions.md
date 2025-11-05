# Clause 3: Definitions

For the purposes of this document, the following terms and definitions apply. Consult the IEEE Standards Dictionary Online for terms not defined in this clause.[^clause3-dictionary]

**access priority** — The priority associated with a transmit request made by a MAC Security Entity (SecY) at its Common Port.

**Association Number (AN)** — A number that is concatenated with the Secure Channel Identifier (SCI) to identify a Secure Association (SA).

**black-side** — Identifies the Ethernet Data Encryption device (EDE) port that uses MACsec to protect transmitted frames and verify received frames.

**bounded receive delay** — A guarantee that a frame will not be delivered after a known bounded time.

> **NOTE** — In the case of the MAC Service, the bound is typically assumed to be less than two seconds.

**Bridged Local Area Network** — A concatenation of individual IEEE 802 Local Area Networks (LANs) interconnected by MAC Bridges.

> **NOTE** — Unless explicitly specified, the term *network* in this standard refers to a Bridged Local Area Network. The term *Local Area Network* and the abbreviation *LAN* refer exclusively to an individual LAN specified by a MAC technology without the inclusion of Bridges. This precision distinguishes a Bridged Local Area Network from an individual LAN that has been bridged to others in the network. The goal of MAC Security is to remain transparent to MAC Service users, so general usage within the standard may not require this distinction.

**Cipher Suite** — A set of one or more algorithms designed to provide any number of the following: data confidentiality, data authenticity, or data integrity.

**Common Port** — An instance of the MAC Internal Sublayer Service (ISS) used by the SecY to provide transmission and reception of frames for both the controlled and uncontrolled ports.

**Controlled Port** — The access point used to provide the secure MAC Service to a client of a MAC Security Entity (SecY).

**cryptographic key** — A parameter that determines the operation of a cryptographic function such as:

- (a) the transformation from plain text to cipher text and vice versa
- (b) synchronized generation of keying material
- (c) digital signature computation or validation[^clause3-digital-signature]

**cryptographic mode of operation** — An algorithm for the cryptographic transformation of data that features a symmetric key block cipher algorithm. Synonym: *mode*.[^clause3-crypto-mode]

**Customer Edge Port** — The red-side port of an Ethernet Data Encryption device (EDE-CS, EDE-CC, or EDE-SS).

> **NOTE** — The terms *customer* and *provider* match the terminology in IEEE Std 802.1Q for Provider Edge Bridges and reflect the roles of the ports in a layered network architecture; they do not indicate equipment ownership. The same terminology is extended to ports with equivalent roles in EDE-CC and EDE-SS devices. Additional variants of Provider Edge Bridges or Backbone Edge Bridges are not implied.

**Customer Network Port** — A port on the network component of an Ethernet Data Encryption device (EDE-CS, EDE-CC, or EDE-SS) that provides internal connectivity to the edge component of that EDE.

**data integrity** — A property whereby data has not been altered in an unauthorized manner since it was created, transmitted, or stored.[11]

**edge component** — The bridge component in an Ethernet Data Encryption device (EDE-CS, EDE-CC, or EDE-SS) that is attached to the red-side port.

**Ethernet Data Encryption device (EDE)** — A two-port bridge that transmits and receives frames—assumed to be unprotected—to and from one red-side port, and conditionally relays those frames to and from its black-side port, protecting and verifying frames on the black-side port using MACsec.

**IEEE 802 Local Area Network (LAN)** — LAN technologies that provide a MAC Service equivalent to the MAC Service defined in IEEE Std 802.1AC. IEEE 802 LANs include IEEE Std 802.3 (Ethernet) and IEEE Std 802.11 (Wireless).[B2]

> **NOTE** — IEEE 802 LANs are also referred to simply as LANs in this standard.

**initialization vector (IV)** — A vector used to define the starting point of an encryption process within a cryptographic algorithm.[^clause3-iv]

**integrity** — See *data integrity*.

**integrity check value (ICV)** — A value derived by performing an algorithmic transformation on the data unit for which data integrity services are provided. The ICV accompanies the protected data unit and is recalculated and compared by the receiver to detect modification.

**key** — See *cryptographic key*.

**key management** — The generation, storage, distribution, deletion, archiving, and application of keys in accordance with a security policy.

**Layer Management Interface (LMI)** — The interface between a protocol entity in a system and the system management, providing for the exchange of parameters with other system entities that are not attached to the service access points used and provided by the protocol entity.

**Local Area Network (LAN)** — See *IEEE 802 Local Area Network (LAN)*.

**MAC Security Entity (SecY)** — The entity that operates the MAC Security protocol within a system.

**MAC Security TAG (SecTAG)** — A protocol header, beginning with an EtherType, that is prepended to the service data unit supplied by the client of the protocol and used to provide security guarantees.

**MAC service data unit (MSDU)** — A sequence of zero or more octets that compose the data to be communicated with a single MAC Service request or indication.

**master key** — A secret key used to derive one or more cryptographic keys that are used directly to protect data transfer.

**message authentication** — A cryptographic guarantee that an authenticated message was not modified in transit and originated from an entity with the proper cryptographic credentials.

**mode** — See *cryptographic mode of operation*.

**multipoint** — Involving, or potentially involving, more than one participant in the role of receiver or transmitter in a single data transfer or a set of related data transfers.

**network component** — The bridge component in an Ethernet Data Encryption device (EDE-CS, EDE-CC, or EDE-SS) that is attached to the black-side port.

**nonce** — A non-repeating value, such as a counter, used in key management protocols to thwart replay and other types of attack.[^clause3-nonce]

**Packet Number (PN)** — A monotonically increasing value that is guaranteed unique for each MACsec frame transmitted using a given Secure Association Key (SAK).

**Port Identifier** — A 16-bit identifier that uniquely identifies each transmit Secure Channel (SC) that uses the same MAC address as a component of its Secure Channel Identifier (SCI).

> **NOTE** — The Port Identifier need not correspond to any other port number, identifier, or index. There can be more than one SC for a physical port—identifying frames transmitted by separate virtual ports—and more than one SC for a physical or virtual port if that port transmits frames of different priorities.

**Protocol Data Unit (PDU)** — A unit of data specified in a protocol and consisting of protocol information and, possibly, user data.

**Provider Edge Port (PEP)** — A port on the edge component of an Ethernet Data Encryption device (EDE-CS, EDE-CC, or EDE-SS) that provides internal connectivity to the network component of that EDE.

**Provider Network Port** — The black-side port of an Ethernet Data Encryption device (EDE-CS, EDE-CC, or EDE-SS).

**red-side** — Identifies the Ethernet Data Encryption device (EDE) port that does not use MACsec to protect transmitted frames or verify received frames.

**Reserved Address** — A group address filtered by a bridge component to restrict the scope of the control protocols using that Destination Address.

**secret key** — A cryptographic key used with a secret-key algorithm that is uniquely associated with one or more entities and should not be made public.[^clause3-secret-key]

**Secure Association (SA)** — A security relationship that provides security guarantees for frames transmitted from one member of a Connectivity Association (CA) to the others. Each SA is supported by a single secret key, or a set of keys when protecting one frame requires multiple keys.

**Secure Association Identifier (SAI)** — An identifier for a Secure Association (SA), comprising the Secure Channel Identifier (SCI) concatenated with the Association Number (AN).

**Secure Association Key (SAK)** — The secret key used by a Secure Association (SA).

**Secure Channel (SC)** — A security relationship used to provide security guarantees for frames transmitted from one member of a Connectivity Association (CA) to the others. An SC is supported by a sequence of Secure Associations (SAs), allowing periodic key refresh without terminating the relationship.

**Secure Channel Identifier (SCI)** — A unique identifier for a Secure Channel (SC), composed of a MAC Address and a Port Identifier.

> **NOTE** — Key agreement protocols such as MACsec Key Agreement (MKA) ensure that each SCI used with a given Secure Association Key (SAK) is unique where required for nonce construction (e.g., the default Cipher Suite in 14.5). SCI uniqueness is independent of MAC Address allocation procedures.

**secure Connectivity Association (CA)** — A security relationship, established and maintained by key agreement protocols, that comprises a fully connected subset of service access points in stations attached to a single LAN and supported by MACsec.

**Short Secure Channel Identifier (SSCI)** — A 32-bit value, managed by the key agreement protocol, that is unique for each SCI within the context of all MAC Security Entities (SecYs) using a given Secure Association Key (SAK).

**spoofing** — Claiming a fraudulent identity for purposes of mounting an attack.

**Uncontrolled Port** — The access point used to provide the insecure MAC Service to a client of a MAC Security Entity (SecY).

**user priority** — The priority associated with a transmit request received by the Controlled Port of a MAC Security Entity (SecY).

[^clause3-dictionary]: The IEEE Standards Dictionary Online supplies normative IEEE 802 terminology referenced throughout this clause.
[^clause3-digital-signature]: Digital signature terminology follows the conventions used in ISO/IEC 14888 and NIST FIPS 186-4.
[^clause3-crypto-mode]: Modes of operation correspond to the block cipher guidance in NIST SP 800-38 series publications.
[^clause3-iv]: MACsec adopts the initialization vector practices defined for GCM in NIST SP 800-38D.
[^clause3-nonce]: Nonce usage aligns with the replay protection guidance found in IEEE 802.1X and NIST SP 800-38D.
[^clause3-secret-key]: Secret key management requirements are consistent with ISO/IEC 11770 and related key management standards.
