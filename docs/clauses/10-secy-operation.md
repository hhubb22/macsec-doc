# Clause 10: Principles of SecY Operation

This clause describes how a MAC Security Entity (SecY) implements the secure MAC Service, including its architecture, runtime processes, and management controls. It links the protocol requirements (Clause 8) and encoding rules (Clause 9) to the behavior expected from a conformant implementation.

## 10.1 SecY Overview

- A SecY consumes the insecure MAC Service via its Common Port and offers:
  - A secure MAC Service instance on the Controlled Port
  - A transparent (insecure) service on the Uncontrolled Port
- Parameters delivered via the Controlled Port must be authenticated (and optionally encrypted). Non-validated frames are discarded.
- Requests and indications on the Uncontrolled Port are mirrored at the Common Port; ordering across ports is preserved per-port but not defined across ports.
- Cipher Suites (Clause 14) dictate the cryptographic protection; at least the default suite must be implemented.
- The co-resident KaY (part of the IEEE 802.1X PAE) uses the Uncontrolled Port for key agreement, sets `MAC_Operational`, and exchanges keys/configuration via the Layer Management Interface (LMI).

## 10.2 SecY Functions

A SecY shall support:

- Secure transmission for Controlled Port requests
- Transparent transmission for Uncontrolled Port requests
- Secure reception/validation for Controlled Port indications
- Transparent reception for Uncontrolled Port indications
- Reporting of MAC status and point-to-point parameters (Clause 6)

Management facilities must allow:

- Operation without frame modification (`protectFrames = False`)
- Receive-only testing (`validateFrames` controls)
- Multiple transmit SCs and configurable replay windows
- KaY discovery of Cipher Suite support and SC capabilities
- KaY provisioning and confirmation of transmit/receive SAKs and PN monitoring
- Counters for discarded/accepted frames across validation failure modes

## 10.3 Model of Operation

The formal model describes externally observable behavior; implementations may use any internal architecture that yields the same service semantics and counter behavior.

## 10.4 SecY Architecture

Key components (see Figure 10-2 placeholder):

- Controlled, Uncontrolled, and Common Ports (with status parameters)
- Secure Frame Generation (10.5)
- Secure Frame Verification (10.6)
- Cipher Suite protect/validate functions
- Transmit Multiplexer and Receive Demultiplexer
- Optional FCS regenerators
- SecY Management process (10.7) accessible via the LMI

Controls enable staged deployment:

- `protectFrames`, `validateFrames`, `replayProtect`, `replayWindow`
- Counters in Figures 10-3/10-4 (placeholders) summarise control effects

Implementations must not increase undetected frame error rates relative to preserving the original FCS (Clause 6.10).

## 10.5 Secure Frame Generation

For each Controlled Port request:

1. **SA Selection (10.5.1)** — Choose the transmit SA based on SC membership and management policies.
2. **Packet Number Handling (10.5.2)** — Use the `nextPN` for the SA, ensuring monotonicity and key-rollover coordination.
3. **SecTAG Construction (10.5.3)** — Populate EtherType, TCI bits, AN, SL (if needed), PN LSBs, and SCI (if required).
4. **Protection (10.5.4)** — Invoke the Cipher Suite’s PROTECT function with SAK, PN, SCI, SecTAG, and user data. Receive Secure Data + ICV output.
5. **Transmission (10.5.5)** — Submit the augmented frame to the Common Port.

Additional points:

- SA selection obeys management constraints (e.g., protectFrames, confidentiality offsets, SCI usage).
- PN handling includes wrap prevention; KaY monitors for exhaustion.
- SL is set whenever the post-SecTAG payload is < 48 octets.
- SCI transmission follows TCI bits and point-to-point rules.

## 10.6 Secure Frame Verification

For each frame from the Common Port:

1. **Eligibility Check (10.6.1)** — Ensure EtherType or MACsec integrity before processing. Apply `validateFrames` policy.
2. **SA Lookup (10.6.2)** — Use AN and SCI (if present) or management-provisioned mapping to locate the receive SA.
3. **PN Reconstruction & Replay Check (10.6.3)** — Reconstruct 64-bit PN if needed. Apply `replayProtect` and `replayWindow` policies to determine acceptance.
4. **Validation (10.6.4)** — Call the Cipher Suite’s VALIDATE function; verify integrity/confidentiality.
5. **Delivery (10.6.5)** — Deliver user data to Controlled Port on success; update counters and PN state. Optionally forward to Uncontrolled Port if policy permits and EtherType matches.

Frames failing any step are discarded, with reasons reflected in management counters.

## 10.7 SecY Management

Management variables and controls (selected highlights):

- **Boolean Controls** — `protectFrames`, `validateFrames` (Strict/Check/Disabled), `replayProtect`, `confidentialityOffset0/30/50`, `canDisablePort`, `alwaysIncludeSCI`, `useES`, `useSCB`.
- **Numeric Controls** — `replayWindow`, `rekeyThreshold`, per-SA `nextPN` and `lowestAcceptablePN`, `defaultPRF` parameters.
- **Status/Capabilities** — Supported Cipher Suites, max SC/SA counts, confidentiality offsets, SCI inclusion support, Replay capabilities, Delay bounds (Table 10-3 placeholder).
- **Counters** — Transmit/receive totals; discards by cause (length, validation, unknown SCI/SA, replay failure, integrity failure); confidentiality offsets in use; policy-driven drops.
- **Events** — PN exhaustion warnings, validate failures, replay alarms, KaY notifications.

The LMI exposes these controls/counters to the KaY and local management (e.g., SNMP). Annex references provide PICS and MIB mappings.

## 10.8 Addressing

- Each transmit SC uses a unique SCI comprising MAC address and Port Identifier.
- SecYs must handle cases such as virtual ports, provider bridge stacks, and MAC address sharing.
- Management controls may restrict acceptance to specific SCI/MAC combinations.

## 10.9 Priority

- MACsec shall preserve user priority where possible. Secure channels may be partitioned by priority to avoid replay-window issues.
- Access Priority Tables map user priority to access priority when confidentiality obscures inner headers.

## 10.10 Performance Requirements

- Processing latency per frame must not exceed bounds in Table 10-3 (placeholder), covering encryption/authentication delays.
- Implementations must switch SAKs within one frame time and accept new keys within one second (aligning with Clause 8 requirements).
- Replay windows and counters must support high-speed operation without loss.

---

**Figure Placeholders** — Figures 10-1 through 10-4 (SecY overview, architecture, management counters) need recreating or substitution with local diagrams.

**Table Placeholder** — Table 10-3 (SecY performance requirements) should be transcribed once source data is cleaned.

