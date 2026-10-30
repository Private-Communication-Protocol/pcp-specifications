# PCP Delivery and Relay Model

**Status:** Draft  
**Version:** 0.1.0-draft

## 1. Purpose

This document describes the responsibilities and security boundaries of optional delivery infrastructure.

PCP does not assume that every deployment uses a relay. Applications may choose direct or relayed delivery depending on their requirements and network conditions.

## 2. Relay role

A relay may forward opaque protocol messages between endpoints.

A relay MAY provide temporary store-and-forward behavior if a future delivery profile defines it.

A relay MUST NOT be required to decrypt end-to-end protected application payloads.

## 3. Trust assumptions

Relays are untrusted with respect to message confidentiality and endpoint identity authentication.

A relay may still be able to:

- Observe network connections to it.
- Observe timing and data volume.
- Observe routing or mailbox identifiers required for forwarding.
- Drop, delay, duplicate, reorder, or replay data.
- Refuse service or become unavailable.

The protocol and client must handle these behaviors safely.

## 4. Store-and-forward requirements

If temporary offline delivery is supported, the specification must define:

- How a recipient mailbox or delivery destination is identified.
- Who can submit messages to it.
- How recipient authorization is established.
- Maximum queue size and message size.
- Expiration and deletion semantics.
- Duplicate and replay behavior.
- Acknowledgement meaning.
- Abuse and resource-exhaustion controls.
- Whether delivery metadata is encrypted or visible to the relay.

A relay's acknowledgement MUST NOT be represented as proof that the recipient decrypted or read the message unless the protocol defines and authenticates that separate event.

## 5. Retention

Relay retention must be explicit and bounded by the selected deployment profile.

PCP should not claim that data has been erased from every backup, log, or infrastructure layer unless that behavior is technically enforced and independently verified.

Application-level conversation retention remains outside PCP.

## 6. Relay discovery and operator model

PCP should permit independently operated infrastructure.

The discovery mechanism must avoid creating an implicit mandatory central directory. Possible models include application-configured relays, operator-published endpoints, or other mechanisms to be evaluated.

Relay authenticity and endpoint authentication must be separate from discovery.

## 7. Abuse resistance

Relays may need controls against spam, flooding, oversized messages, mailbox exhaustion, and unauthorized access.

Such controls should avoid requiring unnecessary identity disclosure. The trade-offs between abuse resistance and privacy must be documented.

## 8. Open decisions

- Whether v0.1 includes offline delivery.
- Mailbox addressing and authorization.
- Relay discovery and trust model.
- Queue expiration and deletion semantics.
- Acknowledgement definitions.
- Abuse prevention and rate limits.
- Multi-relay routing and collusion considerations.
