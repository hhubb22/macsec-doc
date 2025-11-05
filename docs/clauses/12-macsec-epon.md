# Clause 12: MACsec and EPON

MACsec secures Ethernet Passive Optical Network (EPON) deployments by protecting each logical link between the Optical Line Terminal (OLT) and Optical Network Units (ONUs).

- IEEE 802.3 Clauses 64–65 define EPON, where the OLT hosts multiple point-to-point MAC instances plus a Single Copy Broadcast (SCB) MAC.
- Each ONU obtains a dedicated secure MAC Service instance by pairing its MAC entity with a SecY located in the OLT (`Figure 12-1` placeholder). This delivers confidentiality, integrity, and origin authenticity for bi-directional traffic regardless of an attacker's ability to emulate EPON signaling.
- The SCB SAP can be protected by a dedicated Secure Channel (SC); associated SAKs are distributed to all authorized ONUs so that broadcast frames remain authenticated and confidential.
- **Note 1** — Because SCB traffic originates only at the OLT, SAKs for SCB SAs are distributed via the point-to-point MACsec channels between the OLT and each ONU.
- **Note 2** — ONUs may discard SCB frames at the EPON MAC layer, but if accepted (e.g., for bridging/routing), integrity and origin checks must be performed to prevent broadcast-based attacks.

**Figure Placeholder** — Figure 12-1 (MACsec with EPON) to be recreated locally.

