# PCP Security Model

**Status:** Draft  
**Version:** 0.1.0-draft

## 1. Purpose

This document identifies the security goals, assets, adversaries, and threat categories that must guide PCP design and implementation.

It is a starting threat model, not a security audit or proof of security.

## 2. Security goals

PCP aims to provide:

- End-to-end confidentiality of protected application payloads.
- Message integrity and authenticity.
- Authentication of peer cryptographic identities under a defined verification model.
- Replay resistance.
- Forward secrecy where supported by the selected session protocol.
- Secure key lifecycle and storage requirements.
- Resistance to downgrade and malformed-input attacks.
- Minimized trust in relays and transport providers.

The exact guarantees depend on selected protocols and implementation correctness.

## 3. Assets

Assets include:

- Long-term private identity keys.
- Ephemeral and pre-published key material, if used.
- Session keys and ratchet state.
- Plaintext application payloads.
- Peer identity bindings and verification state.
- Pending outbound and inbound messages.
- Replay-protection state.
- Local configuration and transport credentials.
- Metadata that may reveal communication relationships.

## 4. Adversaries

The threat model should consider:

- Passive network observers.
- Active network attackers.
- Malicious or compromised relays.
- Malicious transport providers.
- Malicious peers.
- Attackers able to submit malformed protocol data.
- Local malware or a compromised operating system.
- A compromised application embedding PCP.
- Infrastructure operators who can observe or alter their own systems.
- Colluding relays or observers.

A global passive adversary capable of observing both ends of a low-latency communication path is a particularly strong traffic-analysis threat.

## 5. Out of scope and limitations

PCP cannot by itself protect plaintext from:

- A compromised endpoint.
- A malicious application that receives plaintext.
- A user who copies or shares plaintext.
- Screen capture or photography by an external device.
- Malware with sufficient access to memory or keys.
- Traffic analysis by sufficiently capable observers unless additional defenses are specified and effective.

The protocol must state these limitations clearly.

## 6. Threat categories

The design and test plan must cover:

- Key substitution and impersonation.
- Man-in-the-middle attacks.
- Replay and message reordering.
- Downgrade attacks.
- Ciphertext or header modification.
- Nonce or counter misuse.
- Randomness failure.
- Key leakage and state rollback.
- Malformed serialization and parser differentials.
- Resource exhaustion and denial of service.
- Relay message dropping, duplication, and delay.
- Metadata correlation and traffic analysis.
- Unsafe logging, crash reports, and diagnostic output.
- Insecure backup, migration, or recovery.

## 7. Cryptographic requirements

PCP MUST use established, reviewed cryptographic protocols and libraries.

PCP MUST NOT invent its own encryption algorithm, key exchange, MAC, signature scheme, or random-number generator.

The final design must specify:

- Cryptographic algorithms and parameters.
- Key derivation and domain separation.
- Nonce or counter management.
- Authentication and associated-data rules.
- Key rotation and destruction.
- Forward-secrecy and post-compromise-security properties.
- Post-quantum strategy, if any.
- Required test vectors and known-answer tests.

## 8. Implementation requirements

Implementations SHOULD:

- Fail closed on cryptographic or authentication errors.
- Avoid secret-bearing logs and exception messages.
- Validate input lengths before allocation.
- Use constant-time operations where required by the selected primitives and library guidance.
- Protect private state at rest.
- Use secure randomness.
- Fuzz parsers and state machines.
- Test replay, tampering, malformed input, and state recovery.
- Pin and review cryptographic dependencies.
- Document platform-specific limitations.

## 9. Security review gates

Before a stable release, PCP should require:

1. A reviewed threat model.
2. Documented cryptographic protocol selection and rationale.
3. Known-answer and interoperability test vectors.
4. Negative tests for tampering, replay, malformed input, and downgrade.
5. Fuzzing of parsers and state machines.
6. Dependency and implementation review.
7. Independent security review appropriate to the release's risk.

Passing local tests alone is not evidence of production security.

## 10. Open decisions

- Formal attacker capability model.
- Selected cryptographic protocol suite.
- Secure storage requirements per platform.
- Key compromise and recovery behavior.
- Denial-of-service limits.
- Required independent review scope.
- Security disclosure and vulnerability response process.
