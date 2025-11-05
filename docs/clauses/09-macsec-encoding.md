# Clause 9: Encoding of MACsec Protocol Data Units

This clause defines the structure and encoding of MACsec Protocol Data Units (MPDUs) exchanged between SecYs. It covers field representation rules, the composition of each PDU, per-field semantics, validation rules, and the MACsec EtherType assignment.

> **NOTE** — Validation checks in this clause ensure correct decoding only; SecY operational behavior remains specified in Clause 10.

## 9.1 Structure, Representation, and Encoding

- MPDUs always contain an integral number of octets.
- Octets are numbered from 1 upward in the order they are inserted into the MSDU presented to the ISS.
- Bits within an octet are numbered 1–8, with bit 1 the least significant.
- Diagrams depict lower-numbered octets above or to the left of higher-numbered octets; within an octet, higher-numbered bits appear to the left.
- Consecutive octets shown as hexadecimal place lower-numbered octets to the left, with hyphen separators (e.g., `01-80-C2-00-00-00`).
- For multi-octet binary numbers, the lower-numbered octet holds the more significant bits; for bits within an octet, the higher-numbered bit is more significant. Flags occupy a single bit (1 = True, 0 = False); remaining bits may encode other fields.

## 9.2 Major Components

An MPDU consists of three contiguous components:

1. MAC Security TAG (SecTAG) (9.3)
2. Secure Data (9.10)
3. Integrity Check Value (ICV) (9.11)

> **NOTE** — Source and destination MAC addresses are service parameters, not part of the MPDU. Figure 9-1 illustrates the component layout (placeholder pending).

## 9.3 MAC Security TAG

The SecTAG, identified by the MACsec EtherType, carries:

- TAG Control Information (TCI, 9.5)
- Association Number (AN, 9.6)
- Short Length (SL, 9.7)
- Packet Number (PN, 9.8)
- Optional Secure Channel Identifier (SCI, 9.9)

> **Figure 9-2** — SecTAG format (placeholder).

## 9.4 MACsec EtherType

The EtherType occupies octets 1–2 of the SecTAG and enables coexistence with other protocols, incremental deployment, shared-media operation, and independent key-agreement traffic.

| Tag Type                  | Name              | Value |
|--------------------------|-------------------|-------|
| IEEE 802.1AE Security TAG | MACsec EtherType  | 88-E5 |

> **Figure 9-3** — MACsec EtherType encoding (placeholder).

## 9.5 TAG Control Information (TCI)

Bits 8–3 of octet 3 encode:

- Protocol versioning without altering the EtherType
- Optional use of the MAC Source Address to convey the SCI
- Optional explicit SCI inclusion (7.1.2)
- EPON Single Copy Broadcast support (Clause 12)
- Enablement of confidentiality protection
- Replay protection configuration

> **Figure 9-4** — TCI and AN layout (placeholder).

The association-number bits (AN0–AN1) form the two-bit AN field (9.6). Bit 2 being zero avoids ambiguity with VLAN tags and allows certain provider-bridge optimizations.

## 9.6 Association Number (AN)

- Encoded in octet 4 alongside the first PN octets.
- Identifies which SA protects the frame, enabling receive-side SAK selection.
- May change between frames on an SC without additional notification; frame order remains that of the underlying medium.

## 9.7 Short Length (SL)

- Encoded in the lower 6 bits of octet 4 when the frame carries fewer than 48 octets after the SecTAG.
- Indicates the total octet count from the start of the SecTAG to the end of the ICV, allowing receivers to locate the ICV even when padding is present.
- The full PDU length is `SecTAG_length + SL` when SL ≠ 0; otherwise the length is determined by the medium (≥ 48 octets ensures compatibility with minimum Ethernet frame size).

## 9.8 Packet Number (PN)

- Octets 5–8 carry the 32 least significant bits of the PN.
- PNs are strictly increasing per SA and are unique within the lifetime of an SA.
- Combined with the SCI (explicit or implicit) to form the IV for Cipher Suite operations and to enforce replay protection.
- For extended packet numbering (64-bit PN), the most significant 32 bits are recovered per Clause 10.6. GCM-AES-XPN Cipher Suites derive a 96-bit IV from a Short SCI (SSCI) plus the 64-bit PN.

## 9.9 Secure Channel Identifier (SCI)

When the SC bit is set in the TCI:

- Octets 9–16 encode the SCI (System Identifier + Port Identifier) of the transmitting SecY.
- Supports distinguishing multiple SCs within a CA and aids network management.
- The System Identifier uses a globally unique MAC address in canonical order; the Port Identifier is a 16-bit integer.
- The value `FF-FF-FF-FF-FF-FF-FF` is reserved to indicate absence of an SCI.
- Point-to-point CAs with a single transmit SC may omit the SCI, though the SCI or SSCI still participates in cryptographic calculations.

## 9.10 Secure Data

- Comprises all octets between the SecTAG and the ICV; never zero-length because the MAC Service requires a non-null MSDU.
- Receivers may discard very short MSDUs (e.g., < 2 octets) per higher-layer expectations.
- The SL field ensures receivers can identify the ICV boundary even when intermediate devices pad frames.

## 9.11 Integrity Check Value (ICV)

- Cipher Suite dependent length, between 8 and 16 octets.
- Protects destination and source MAC addresses, the entire SecTAG, and Secure Data contents.

## 9.12 PDU Validation

A received MPDU is valid only if all conditions hold:

1. Total length ≥ 17 octets.
2. Octets 1–2 equal the MACsec EtherType (`88-E5`).
3. The TCI V bit is clear (0).
4. If ES or SCB bits are set, the SC bit is clear.
5. Bits 7 and 8 of octet 4 are clear.
6. If C = 0 and SC = 0, the length equals 24 + SL (when SL ≠ 0) or ≥ 72 octets otherwise.
7. If C = 0 and SC = 1, the length equals 32 + SL (when SL ≠ 0) or ≥ 80 octets otherwise.
8. If C = 1 and SC = 0, the length equals 8 + ICV_length + SL (when SL ≠ 0) or ≥ 48 + ICV_length otherwise.
9. If C = 1 and SC = 1, the length equals 16 + ICV_length + SL (when SL ≠ 0) or ≥ 48 + ICV_length otherwise.

Frames failing any condition are rejected prior to Cipher Suite validation.

---

**Figure Placeholders** — Figures 9-1 through 9-4 and related diagrams will be recreated or referenced locally in a later pass.

