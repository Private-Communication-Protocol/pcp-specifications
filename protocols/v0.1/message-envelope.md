# PCP Message Envelope

**Status:** Draft  
**Version:** 0.1.0-draft

## 1. Purpose

This document defines the logical requirements for a PCP message envelope. It does not define a final byte-level encoding or cryptographic construction.

The envelope is the protocol-level container used to carry protected application payloads and the minimum information needed for processing and delivery.

## 2. Design goals

The envelope SHOULD:

- Be compact and unambiguous.
- Support version identification.
- Bind protected content to the correct session context.
- Enable integrity verification and replay handling.
- Avoid unnecessary plaintext metadata.
- Be transport-independent.
- Be extensible without ambiguous parsing.

## 3. Logical message contents

A future envelope format may need to represent the following logical information:

| Field category | Purpose | Draft status |
|---|---|---|
| Protocol/version marker | Identify the envelope format | Required concept; encoding open |
| Message type | Distinguish protocol message classes | Required concept; values open |
| Session reference | Associate the message with session state | Design open; privacy review required |
| Cryptographic header | Carry fields required by the selected crypto protocol | Defined by selected crypto protocol |
| Protected payload | Encrypted and authenticated application data | Required |
| Delivery metadata | Support forwarding where necessary | Minimize; exact fields open |
| Extension data | Future compatible additions | Rules open |

This table is conceptual. It MUST NOT be interpreted as a proposed serialized field order.

## 4. Protected payload

Application payloads MUST be protected by the selected end-to-end cryptographic protocol before being exposed to untrusted transport or relay infrastructure.

The cryptographic construction must authenticate all security-relevant header information through the selected protocol's defined associated-data or equivalent mechanism.

The envelope format must not permit unauthenticated metadata to alter message interpretation in a security-sensitive way.

## 5. Message identifiers and replay handling

The final design must specify how messages are distinguished and how duplicates or replays are detected.

It must consider:

- Duplicate delivery.
- Delayed delivery.
- Out-of-order delivery.
- Reconnection and retry.
- Counter exhaustion or reset.
- State loss and restoration.
- Malicious replay attempts.

A message identifier is not automatically a privacy-neutral field. Stable or predictable identifiers may enable correlation.

## 6. Parsing and validation

Implementations MUST:

- Reject malformed envelopes.
- Enforce documented size limits.
- Reject unsupported critical fields.
- Avoid ambiguous parsing.
- Validate lengths before allocating large buffers.
- Treat all received data as untrusted.
- Avoid processing payload content before cryptographic authentication succeeds.

Unknown extension behavior must be explicitly specified before a stable release.

## 7. Serialization

The canonical serialization format remains open.

Selection criteria include:

- Cross-language interoperability.
- Deterministic encoding where required.
- Safe parsing.
- Compactness.
- Explicit length handling.
- Versioning and extension support.
- Availability of mature libraries.

A serialization format MUST NOT be selected solely for convenience in the first Python implementation.

## 8. Fragmentation and size limits

The specification must define maximum envelope and payload sizes, and whether fragmentation is supported.

If fragmentation is supported, the design must address reassembly limits, resource exhaustion, ordering, authentication, and privacy implications.

## 9. Open decisions

- Canonical encoding.
- Exact envelope fields and their visibility.
- Message type registry.
- Message identifiers and replay windows.
- Maximum message sizes.
- Fragmentation and reassembly.
- Extension and critical-field rules.
- Test vectors for valid and invalid envelopes.
