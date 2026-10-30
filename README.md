# PCP Protocol v0.1 Specifications

**Status:** Draft  
**Version:** 0.1.0-draft  
**Purpose:** Index of the language-independent PCP v0.1 specification documents.

These documents describe the intended architecture, responsibilities, requirements, and open design questions for the Private Communication Protocol (PCP).

> **Draft warning:** These documents are not a finalized protocol standard. Any field layout, algorithm, handshake, identifier format, or wire behavior described as proposed or illustrative is not normative. Security-sensitive choices require explicit evaluation, review, and test vectors before implementation.

## Documents

| Document | Purpose |
|---|---|
| [Overview](overview.md) | Project goals, scope, and design principles |
| [Architecture](architecture.md) | Components, boundaries, and data flow |
| [Identity](identity.md) | Cryptographic identities, keys, and identity lifecycle |
| [Session Establishment](session-establishment.md) | Requirements for authenticated session setup |
| [Message Envelope](message-envelope.md) | Logical message structure and processing requirements |
| [Transport](transport.md) | Transport abstraction and network requirements |
| [Delivery and Relay](delivery-and-relay.md) | Optional relay, queueing, and delivery responsibilities |
| [Security Model](security-model.md) | Assets, adversaries, threats, and security requirements |
| [Privacy Model](privacy-model.md) | Metadata, observability, and privacy limitations |
| [Versioning and Interoperability](versioning-and-interoperability.md) | Compatibility, negotiation, and implementation conformance |

## Normative language

The terms **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY** are intended to have their usual standards-document meanings. In this draft, they express proposed requirements unless a section is explicitly marked normative in a future stable release.

## Current design status

PCP is intended to be:

- Language-independent at the specification layer.
- Implementable in multiple languages.
- Modular across cryptography, identity, message handling, and transport.
- Usable with independently operated infrastructure.
- Designed to minimize unnecessary metadata exposure.
- Based on established cryptographic protocols and reviewed implementations.

The first reference implementation is planned in Python. That implementation choice does not define the protocol.

## Open decisions

The following are deliberately not finalized here:

- Exact identity and public-key encoding.
- Exact authenticated key agreement and session protocol.
- Ratchet and key lifecycle details.
- Cipher suite and cryptographic library selection.
- Canonical serialization and binary wire encoding.
- Message identifier and replay-window formats.
- Relay discovery, routing, and mailbox protocol.
- Padding, batching, cover traffic, and traffic-analysis defenses.
- Formal conformance and test-vector format.

These decisions should be recorded separately with rationale and review status.
