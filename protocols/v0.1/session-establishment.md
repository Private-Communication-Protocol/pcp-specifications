# PCP Session Establishment

**Status:** Draft  
**Version:** 0.1.0-draft

## 1. Purpose

This document defines the security properties and functional requirements for establishing a protected session between PCP endpoints.

It does not select a specific handshake or key agreement protocol.

## 2. Goals

Session establishment SHOULD provide:

- Mutual or explicitly defined peer authentication.
- Confidentiality and integrity for subsequent messages.
- Key separation between sessions and purposes.
- Forward secrecy where supported by the selected protocol.
- Replay resistance.
- Explicit handling of identity changes and key compromise.
- Safe recovery from interrupted or duplicated handshake messages.

## 3. Authentication requirements

Before a session is considered authenticated, the implementation MUST validate peer identity material according to the selected identity-verification model.

The specification must distinguish:

- Cryptographic proof that a peer controls a private key.
- Binding of that key to an identity.
- User or application verification that the identity belongs to the intended person or device.

These are not interchangeable.

## 4. Key agreement

The final design MUST select an established, reviewed authenticated key agreement protocol or a reviewed secure messaging protocol that includes session establishment.

PCP MUST NOT invent a custom key exchange.

The selection process must evaluate:

- Authentication model.
- Forward secrecy.
- Post-compromise security.
- Replay and downgrade resistance.
- Asynchronous/offline initiation requirements.
- Multi-device behavior.
- Post-quantum considerations.
- Implementation maturity and independent review.
- Availability of test vectors and interoperable implementations.

## 5. Session state

A session implementation is expected to maintain state such as:

- Peer identity binding.
- Session cryptographic state.
- Sending and receiving key state.
- Message counters or equivalent replay state.
- Protocol version and negotiated capabilities.
- Pending handshake or recovery state.

The exact state format MUST be defined by the selected protocol.

Secret state MUST be protected at rest. It MUST NOT be written to ordinary diagnostic logs.

## 6. Handshake lifecycle

The final specification must define:

1. Initial session initiation.
2. Peer authentication.
3. Key agreement and derivation.
4. Session confirmation.
5. Transition to protected message exchange.
6. Handling of duplicate, delayed, malformed, and out-of-order handshake messages.
7. Session expiration, replacement, and recovery.

This list describes required lifecycle coverage, not a prescribed handshake sequence.

## 7. Asynchronous operation

PCP may be used when both endpoints are not online simultaneously.

The session design must explicitly consider asynchronous initiation, one-time or prepublished key material if used, depletion and replenishment, and malicious replay or reuse of setup material.

Any server or relay involved in distributing public setup material must not be able to substitute peer identity keys without detection under the chosen authentication model.

## 8. Downgrade resistance

Implementations MUST reject unsupported or disallowed security modes rather than silently falling back to weaker cryptographic behavior.

Version and capability negotiation must be authenticated or cryptographically bound to the session establishment process.

## 9. Failure behavior

Session establishment MUST fail closed when authentication, key validation, cryptographic verification, or required state persistence fails.

Implementations MUST NOT continue by silently creating an unauthenticated session.

Errors exposed to applications should be useful without leaking secrets or sensitive internal state.

## 10. Open decisions

- Selected session protocol.
- Initial and recurring key agreement.
- Ratchet design and state transitions.
- Asynchronous setup mechanism.
- Post-quantum migration strategy.
- Session reset and recovery behavior.
- Authentication UX and verification requirements.
