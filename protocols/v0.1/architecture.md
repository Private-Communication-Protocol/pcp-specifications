# PCP Architecture

**Status:** Draft  
**Version:** 0.1.0-draft

## 1. Purpose

This document defines the logical architecture of the Private Communication Protocol (PCP). It identifies the major components, their responsibilities, and the boundaries between PCP and the applications or infrastructure that use it.

The architecture is independent of programming language, operating system, hosting provider, and specific network transport.

## 2. Architectural goals

PCP SHOULD:

1. Separate protocol logic from application logic.
2. Separate cryptographic processing from transport.
3. Allow multiple independent implementations to interoperate.
4. Avoid requiring a single central PCP-operated service.
5. Minimize the information exposed to relays and network intermediaries.
6. Permit different transport mechanisms without changing the application-facing protocol model.
7. Make security-critical boundaries explicit and testable.

## 3. Logical components

### 3.1 Application integration interface

The application integration interface connects an application to a PCP implementation.

It MAY expose operations such as:

- Create or load a local cryptographic identity.
- Establish or resume a secure session.
- Submit application payloads for protected transmission.
- Receive and authenticate protected payloads.
- Configure an application-selected transport.
- Observe delivery outcomes exposed by the selected deployment.

It MUST NOT imply that PCP owns application accounts, profiles, contacts, groups, or user-interface policy.

### 3.2 Identity component

The identity component manages cryptographic identity material and identity-related state.

Responsibilities include:

- Creating or importing supported identity material.
- Protecting private keys at rest through the host environment or a defined secure-storage integration.
- Exposing public identity material when required by the protocol.
- Supporting identity rotation and revocation mechanisms once specified.
- Preventing private key material from being exposed to relays.

The exact identity model and encoding remain open.

### 3.3 Session and cryptographic component

This component establishes and maintains cryptographic sessions between endpoints.

Its responsibilities are expected to include:

- Authenticated key establishment.
- Session-key derivation and lifecycle.
- Message encryption and authentication.
- Replay and tampering protection.
- Forward-secrecy and post-compromise-security properties where supported by the selected protocol.
- Safe handling of session state and key updates.

PCP MUST use established, reviewed cryptographic protocols and implementations. It MUST NOT define custom cryptographic primitives.

The exact protocol and library selection are open.

### 3.4 Message processing component

The message processing component converts application payloads into protocol messages and validates received messages.

It is responsible for:

- Applying the defined envelope rules.
- Associating protected payloads with the correct session context.
- Validating required fields and limits.
- Rejecting malformed, unauthenticated, or invalid messages.
- Passing successfully authenticated plaintext to the local application boundary.

It MUST NOT interpret application-specific payload semantics unless a future extension explicitly defines them.

### 3.5 Transport adapter

The transport adapter moves protocol data between endpoints or infrastructure components.

Possible transports may include direct network connections, relays, onion-routed paths, or other supported mechanisms.

Transport adapters MUST NOT need access to plaintext application payloads or endpoint private keys.

Transport selection and transport-specific privacy properties MUST be documented separately.

### 3.6 Optional relay and delivery infrastructure

A deployment MAY use independently operated relays or temporary delivery infrastructure.

Such infrastructure may forward opaque protocol data, temporarily queue it, or perform other explicitly specified delivery functions.

A relay MUST NOT be treated as a trusted endpoint for message confidentiality. The relay's visibility into routing identifiers, timing, size, and connection metadata must be documented.

### 3.7 Local storage adapter

A PCP implementation may require local storage for identity keys, session state, pre-keys, replay state, and pending outbound messages.

The storage adapter is local to an endpoint implementation. Its security requirements and lifecycle must be defined by the relevant specifications.

PCP does not define an application's long-term conversation database or retention policy.

## 4. High-level data flow

A typical protected outbound message is expected to follow this logical flow:

1. The application supplies a payload to its local PCP integration.
2. The session component selects the appropriate authenticated session state.
3. The message component constructs the protocol message.
4. The cryptographic component encrypts and authenticates the payload.
5. The transport adapter sends the resulting opaque data.
6. Optional infrastructure forwards or temporarily queues it.
7. The receiving endpoint validates the message, updates session state, and decrypts it.
8. The receiving application receives the authenticated payload.

This is a logical sequence, not a finalized wire-level procedure.

## 5. Trust boundaries

The architecture distinguishes at least these boundaries:

- **Application to PCP library:** The application can see its own plaintext payloads. A compromised application can expose them.
- **PCP library to local storage:** Stored private keys and session state require protection against local compromise.
- **Endpoint to transport:** Transport providers may observe network metadata.
- **Endpoint to relay:** Relays should receive only the information required for their defined forwarding role.
- **Relay to relay:** Any multi-hop design must specify what each hop can observe and what collusion risks remain.

## 6. Application responsibilities

Applications remain responsible for:

- User accounts and authentication policy.
- Contact discovery and contact authorization.
- Group membership and group semantics.
- Usernames, profiles, and presentation.
- Application-level control-event meaning.
- User consent and permissions.
- Local plaintext handling and device security.
- Application-specific storage and retention.
- Abuse handling and moderation policy.

PCP may provide mechanisms for securely transporting application-defined control messages, but does not own their meaning.

## 7. Deployment model

PCP SHOULD support deployments in which applications choose and operate their own infrastructure or use independently operated infrastructure.

No mandatory central PCP messaging service is assumed.

The protocol must not depend on a single vendor's account system, cloud, domain, or network.

## 8. Open architecture decisions

- Exact API boundary between application and PCP.
- Whether the first release defines a single-process library interface, a local daemon interface, or both.
- Relay discovery and routing.
- Offline delivery and mailbox semantics.
- Group communication support and whether it belongs in a later protocol version.
- Multi-device identity and session relationships.
- Local storage interface and backup semantics.
