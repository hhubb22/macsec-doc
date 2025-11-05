# Clause 8: MAC Security Protocol (MACsec)

MACsec delivers the secure MAC Service (Clause 6) on a frame-by-frame basis using cryptographic protection defined within security relationships established by MACsec Key Agreement (MKA, IEEE Std 802.1X).

This clause:

- Defines protocol design requirements (8.1)
- Specifies supporting-system requirements (8.2)
- Provides an operational overview (8.3)

> **NOTE 1** — KaY operation is outside the scope of this standard, but the relationships it establishes (Clause 7) are essential to MACsec.

Conformance is evaluated against the observable behavior of a MAC Security Entity (SecY, Clause 10), including its management interface and client service.

Each set of cryptographic algorithms used by MACsec constitutes a Cipher Suite. This clause focuses on SecY behavior when applying a Cipher Suite (see Figure 8-1, placeholder pending); Clause 14 gives the normative cipher definitions. Cipher Suite selection occurs during CA establishment (7.1.1).

> **NOTE 2** — Figure 8-1 depicts Destination and Source Address parameters separately because they are distinct MAC Service parameters. Actual frame encoding remains the responsibility of the underlying medium.

## 8.1 Protocol Design Requirements

MACsec supports secure communication across networks of end stations and LANs interconnected by bridges, VLAN-aware bridges, and routers. It preserves all aspects of the secure MAC Service (Clause 6) while meeting requirements for:

- Connectivity (6.7)
- Security (6.8, 6.9, 8.1.1)
- Manageability (8.1.2)
- Interoperability (8.1.3)
- Deployment (8.1.4)
- Coexistence (8.1.5)
- Scalability (8.1.6)
- Intrusion detection (8.1.7)
- Localization and isolation of attacks (8.1.8)
- Implementation flexibility (8.1.9)

MACsec operation (8.3) and the supporting requirements on architecture (Clause 11), Cipher Suites (Clause 14), SecYs (8.2), and Key Agreement protocols collectively satisfy these goals.

### 8.1.1 Security Requirements

The protocol design:

- Enables a succession of SAs—each with its own SAK—to maintain SC connectivity. Perfect Forward Secrecy in key agreement protects against single-key compromise without service interruption.
- Ensures fresh SAs supporting an existing CA can be deployed within one second (8.1.9) and changed as frequently as once every 10 seconds after the KaY supplies the SAK.
- Allows key agreement to operate independently of the SecY so that fresh SAKs can be delivered at any time.

Security ultimately relies on Cipher Suite guarantees (Clause 14), their cryptographic modes, and procedures that keep keys secret.

### 8.1.2 Manageability Requirements

MACsec must not diminish existing administrative capabilities. Configuration and monitoring protocols continue to operate, and existing filtering mechanisms remain applicable. When confidentiality is enabled, policy still controls access to MACsec-secure streams.

Where a shared-media CA or point-to-point CA traverses shared facilities, MACsec can convey the SCI (7.1.2, 8.2.1, 9.9) so that recipients and management systems can identify the transmitting SecY.

### 8.1.3 Interoperability Requirements

Interoperability is achieved via:

- Mandatory support for the Default Cipher Suite
- Cipher Suite specifications that reduce algorithm permutations (Clause 14)
- Encodings that avoid medium-specific assumptions—e.g., explicit Secure Data length to accommodate non-Ethernet media

MACsec must interoperate across a wide range of data rates (1 Mb/s to 100 Gb/s) and media without altering protocol formats. Transmitting SecYs cannot rely on media-specific behavior or padding conventions.

### 8.1.4 Deployment Requirements

MACsec can be introduced incrementally, one LAN at a time. SecY controls (Clause 10) allow devices to be deployed before security is enforced. Administrators can disable integrity checking (Default Cipher Suite) for testing, and management counters confirm connectivity prior to enabling protection.

### 8.1.5 Coexistence Requirements

MACsec allows other protocols to operate on the same LAN, supporting:

- Incremental deployment (8.1.4)
- Key agreement protocols with independent formats
- Shared-media services that coexist with MACsec-protected traffic

### 8.1.6 Scalability Requirements

Resource usage per station depends only on the SecY peers sharing the LAN, independent of other network nodes.

### 8.1.7 Unauthorized Access Attempts

Integrity and replay protection, plus management counters (10.7), expose attempted tampering or replay. Client policy management (7.3) should also record violations.

### 8.1.8 Localization and Isolation of Attacks

By discarding frames from unauthenticated stations, MACsec constrains unauthorized traffic to a single LAN. Authorization policies can further limit impact on control protocols, and MACsec records aid in tracing unauthorized behavior permitted by higher-level policies.

### 8.1.9 Implementation

SecY processing is asynchronous relative to the KaY. Key agreement and SAK changes need not align with frame service events; implementations allow up to one second for KaY scheduling, key computation, and SecY preprocessing.

## 8.2 Protocol Support Requirements

Supporting MACsec imposes requirements on both the secure system hosting the SecY and the Key Agreement protocols:

- **System requirements**
  - SC identification (8.2.1)
  - Transmit/receive SAK support (8.2.2)
- **Key Agreement/KaY requirements**
  - Independence from SecY state (8.2.3)
  - Connectivity discovery (8.2.4)
  - Mutual authentication (8.2.5)
  - Authorization (8.2.6)
  - Key exchange and maintenance (8.2.7)

### 8.2.1 SC Identification Requirements

Each SecY identifies its transmit SCs with an SCI composed of a unique MAC address and a Port Identifier unique within that address (7.1.2, 9.9). MKA confirms SCI uniqueness before secure communication starts.

### 8.2.2 SA Key Requirements

Cipher Suite implementations shall:

- Prepare new SAKs for use within one second of KaY delivery
- Switch from one transmit SAK to the next within a frame time without discarding frames
- Receive any frame and its successor using either of two SAKs to accommodate seamless switchover
- Prepare new receive SAKs within one second of KaY delivery

Implementations need not switch seamlessly between different Cipher Suites.

### 8.2.3 KaY Independence of MACsec

The KaY tracks required connectivity and SC identifiers independently of the SecY design and state, operating resiliently against anticipated denial-of-service scenarios. Distinct EtherTypes separate key agreement frames from MACsec frames.

### 8.2.4 Discovering Connectivity

The KaY:

- Detects peer connections and potential connections
- Uses MAC status parameters (6.4) to identify connectivity changes and can temporarily set status FALSE when authentication/authorization changes
- Learns SecY Cipher Suite and connectivity capabilities via the LMI and communicates selections back to the SecY

### 8.2.5 Authentication Requirements

The Port Access Entity (PAE) provides mutual authentication; the SecY assumes peers are authenticated prior to exchanging protected frames.

### 8.2.6 Authorization Requirements

The PAE authorizes services offered to each peer and informs local services about the chosen Cipher Suite.

### 8.2.7 Key Exchange and Maintenance

Key agreement protocols must supply fresh SAKs, maintain key lifetimes, and coordinate rekeying across the CA while respecting the timing bounds defined in 8.1.9.

## 8.3 MACsec Operation

MACsec augments each transmitted frame with a SecTAG and Integrity Check Value (ICV) as illustrated in Figure 8-2 (placeholder pending). The SecTAG identifies the protocol, associated key, and replay context; the Secure Data field carries user data (optionally encrypted); the ICV covers the MAC addresses, SecTAG, and user data.

> **NOTE 1** — The added overhead can increase frame size beyond media limits. If the underlying service cannot transmit the enlarged MPDU, the frame is discarded.

MACsec does not introduce additional keepalive or key-exchange frames; it processes only client data and control frames carried through the Controlled or Uncontrolled Ports.

### 8.3.1 Transmission

1. Assign the frame to a transmit SC and SA (identified by AN).
2. Retrieve the SAK and next Packet Number (PN) for that SA.
3. Encode the SecTAG with the EtherType, AN, SCI (optional for point-to-point single-SC CAs), PN (least significant 32 bits), and Short Length (SL) if the protected payload is shorter than 48 octets.
4. Invoke the Cipher Suite’s PROTECT function (14.1) with the SAK, PN, SCI, MAC addresses, SecTAG, and user data to produce Secure Data and the ICV.

### 8.3.2 Reception

1. Extract AN, SCI, PN, and SL (if present) from the SecTAG. For point-to-point CAs without SCI in the frame, use the value supplied by the KaY.
2. Identify the SA via AN and SCI, obtaining the corresponding SAK. For extended packet numbering, reconstruct the full 64-bit PN using the previously stored high-order bits.
3. Invoke the Cipher Suite’s VALIDATE function with the SAK, PN, SCI, MAC addresses, SecTAG, Secure Data, and ICV. If validation succeeds, recover the user data and return a VALID indication.
4. Apply replay protection (if enabled) by ensuring the PN is not below the lowest acceptable value. Update the replay window and deliver the user data unchanged to the MACsec client.

> **NOTE 2** — GCM-AES-XPN Cipher Suites (14.7, 14.8) derive a 96-bit IV from a Short Secure Channel Identifier (SSCI) supplied by the KaY plus the 64-bit PN.

Field formats and encoding details for the SecTAG appear in Clause 9; Clause 10 describes SecY behavior, management controls, error handling, and diagnostics.

