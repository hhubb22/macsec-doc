# Clause 14: Cipher Suites

This clause summarizes the set of Cipher Suites available to MACsec, their capabilities, and conformance requirements.

## 14.1 Cipher Suite Use

- A Cipher Suite defines the cryptographic algorithms and parameters used for PROTECT and VALIDATE operations (Clause 8.3).
- SecYs must implement the default suite (GCM-AES-128) and may support additional suites meeting Clause 14 criteria.

## 14.2 Cipher Suite Capabilities

Each suite shall:

- Provide integrity protection via GCM with 128-bit or 256-bit keys.
- Support replay protection through Packet Numbers and optional extended packet numbering (XPN).
- Define confidentiality behavior (enabled/disabled, offsets supported).
- Specify management exposure (e.g., confidentiality offset ranges, replay window limits).

## 14.3 Cipher Suite Specification Criteria

- Suites must rely on NIST-approved algorithms (e.g., AES-GCM).
- IV construction must guarantee uniqueness per SAK/SA.
- Confidentiality offsets, if present, must align with upper-layer requirements (e.g., preserving headers).
- Suites must support SAK rekeying within 1s and PN rollover protections per Clause 8.

## 14.4 Cipher Suite Conformance

- Full conformance requires support for the default suite and adherence to Clause 14 selection criteria.
- Conformance with Cipher Suite variance permits additional suites, provided they satisfy 14.2–14.3.

## 14.5 Default Cipher Suite — GCM-AES-128

- Integrity and optional confidentiality with 128-bit keys.
- 32-bit Packet Number; IV formed from SCI and PN.
- Supports confidentiality offsets of 0, 30, 50 (octets) as configured.

## 14.6 GCM-AES-256

- Mirrors default suite with 256-bit keys for higher security margins.

## 14.7 GCM-AES-XPN-128

- Adds extended packet numbering (64-bit PN) to increase protected-frame limits.
- Uses SSCI + PN to build a 96-bit IV.
- Requires distribution of SSCI and salt via MKA.

## 14.8 GCM-AES-XPN-256

- Combines 256-bit keys with extended packet numbering, mirroring 14.7 otherwise.

**Tables/Figures Pending** — Any Cipher Suite capability tables in the source will be reformatted later.

