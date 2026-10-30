# PCP Versioning and Interoperability

**Status:** Draft  
**Version:** 0.1.0-draft

## 1. Purpose

This document defines requirements for protocol evolution and interoperability between independent PCP implementations.

The objective is to allow separately developed implementations to communicate safely without relying on shared source code, language, vendor, or hosting infrastructure.

## 2. Language independence

The protocol specification is authoritative for wire behavior and security requirements.

An implementation MUST NOT introduce language-specific behavior that changes protocol semantics.

The first reference implementation is planned in Python. Future implementations may use other languages, including Rust, without changing the protocol definition.

## 3. Version identifiers

The final design must define how versions are represented in:

- Protocol specifications.
- Message envelopes.
- Session establishment.
- Optional extensions.
- Capability negotiation.

The exact version field and encoding remain open.

## 4. Compatibility principles

The protocol must distinguish:

- **Backward compatibility:** A newer implementation can communicate with an older supported version.
- **Forward compatibility:** An older implementation can safely handle certain data from a newer version.
- **Wire compatibility:** The encoded data is interpreted identically.
- **Security compatibility:** Negotiation does not weaken required security properties.

Compatibility MUST NOT override mandatory security requirements.

## 5. Negotiation and downgrade protection

If versions or capabilities are negotiated, the negotiation result must be authenticated or cryptographically bound to the established session.

Implementations MUST NOT silently fall back to a weaker or disallowed security profile.

Unknown critical features MUST cause a safe failure. The behavior for unknown non-critical extensions must be explicitly defined.

## 6. Extensions

Extensions should have:

- A defined namespace or identifier.
- A clear criticality rule.
- Defined parsing and validation behavior.
- Documented security and privacy implications.
- A compatibility policy.
- Test vectors where applicable.

Extensions MUST NOT redefine existing fields ambiguously.

## 7. Interoperability requirements

Independent implementations should be able to verify:

- Identical interpretation of protocol fields.
- Compatible identity and session establishment.
- Correct encryption and decryption behavior.
- Correct replay and error handling.
- Consistent version negotiation.
- Consistent handling of malformed and unsupported messages.

## 8. Conformance testing

A future conformance suite should include:

- Valid message vectors.
- Invalid and malformed message vectors.
- Cryptographic known-answer tests.
- Handshake and session-state test cases.
- Replay and out-of-order delivery tests.
- Version negotiation tests.
- Cross-implementation integration tests.

Test vectors should be versioned and published with the specification.

## 9. Deprecation and breaking changes

The project should publish a documented policy for:

- Deprecating fields or algorithms.
- Introducing mandatory security updates.
- Supporting older versions.
- Handling emergency cryptographic changes.
- Announcing incompatible changes.

Security-critical fixes may require faster migration than ordinary feature changes.

## 10. Open decisions

- Version syntax and placement.
- Supported-version policy.
- Negotiation format.
- Extension registry governance.
- Conformance levels.
- Test-vector format and publication process.
- Deprecation and migration policy.
