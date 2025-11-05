# Clause 13: MAC Security Entity (SecY) MIB

This clause defines an SMIv2 MIB module for managing MACsec SecY instances. It aligns with IEEE 802.1AC/802.1Q architecture, interfaces with IF-MIB (RFC 2863), and provides objects for configuration, status, and statistics across Controlled and Uncontrolled Ports.

## 13.1 Introduction

- Establishes an SMIv2-compliant module supporting SNMP-based management of SecYs.
- Implements objects that reflect Clause 10/11 behavior (SecY architecture, frame generation/verification, SC/SA lifecycle).

## 13.2 Internet-Standard Management Framework

- References RFC 3410 for SNMP framework overview and RFCs 2578/2579/2580 for SMIv2 definitions.
- MIB objects are accessible via SNMP using standard Get/Set semantics.

## 13.3 Relationship to Other MIBs

- **System group (RFC 3418 / 1213)** — Expected to be implemented alongside this module.
- **Interfaces MIB (RFC 2863)** — Required; Controlled and Uncontrolled Ports are modelled as sublayer interfaces (`macSecControlledIF(231)`, `macSecUncontrolledIF(232)`). Figure 13-1 (placeholder) illustrates the interface stack.
- Existing IF-MIB objects (e.g., `ifDescr`, `ifType`, `ifSpeed`) apply; SecY-specific attributes are provided in dedicated tables.

## 13.4 Security Considerations

- Highlights the need to protect SNMP exchanges (e.g., SNMPv3 with authentication/privacy).
- Advises that key material and counters revealing traffic patterns be secured appropriately.

## 13.5 Structure of the MIB Module

Major groups (tables to be fully transcribed later):

1. **`macSecMIB` scalars** — Module identity, revision history, notifications.
2. **`macSecObjects`** — Configuration/status objects for: 
   - Interface bindings (`macSecControlledPortTable`, `macSecUncontrolledPortTable`).
   - Frame generation/verification controls (`macSecSciTable`, `macSecCipherSuiteTable`, `macSecSecYStatus` objects).
   - Replay protection parameters (`macSecReplayProtect`, `macSecRekeyThreshold`).
3. **Statistics tables** — Counts for transmitted/received frames, validation failures, PN rollover events.
4. **Notifications** — e.g., PN exhaustion, validation anomalies.
5. **Compliance/statements** — `macSecCompliances`, `macSecGroups` describing mandatory/optional object sets.

> **Tables Pending Conversion** — Detailed tabular definitions (Tables 13-1 onward) will be reformatted into Markdown/SNMP syntax in a future pass.

## 13.6 MIB Definitions (Overview)

- The full SMIv2 module includes macro definitions (`MODULE-IDENTITY`, `OBJECT-TYPE`, `MACRO` statements) covering all objects cited above.
- Compliance statements cover legacy (`secyMIBCompliance`) versus updated (`secyTcMIBCompliance`) groupings; superscript annotations in the OCR identify deprecated vs. recommended objects.
- Textual conventions (e.g., `MacSecDesiredAdminStatus`, `MacSecSCI`, `MacSecCipherSuite`) define data types used across tables.

**Figure Placeholder** — Figure 13-1 (MACsec Interface Stack) to be recreated.

**Next Actions**

- Convert SMIv2 code blocks into Markdown fenced blocks or separate `.mib` files.
- Reformat Tables 13-1 through 13-8 into Markdown for readability.

