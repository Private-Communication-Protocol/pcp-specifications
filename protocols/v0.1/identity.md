# PCP Identity Model

**Status:** Draft  
**Version:** 0.1.0-draft

## 1. Purpose

This document defines requirements and open questions for cryptographic identities used by PCP endpoints.

A PCP cryptographic identity is not the same thing as an application account, username, profile, phone number, or contact record.

## 2. Design principles

The identity model SHOULD:

- Be independent of application account systems.
- Avoid requiring a global public user directory.
- Support independent endpoint implementations.
- Keep private key material under endpoint control.
- Permit key rotation and recovery policies to be defined explicitly.
- Avoid exposing unnecessary stable identifiers to network intermediaries.
- Support authentication of peer key material without assuming that a server is inherently trustworthy.

## 3. Identity terminology

### 3.1 Endpoint

An endpoint is a running PCP-capable client instance that can send or receive protocol messages.

### 3.2 Cryptographic identity

A cryptographic identity is the set of cryptographic material and associated state used to identify or authenticate an endpoint or an identity owner under the selected identity scheme.

The exact composition is not yet specified.

### 3.3 Public identity material

Public identity material is information that may be shared to allow another endpoint to authenticate or initiate a secure session.

Its encoding, distribution, and authenticity mechanism remain open.

### 3.4 Private identity material

Private identity material includes secret keys and related secrets. It MUST NOT be transmitted to relays or remote infrastructure.

## 4. Identity scope

PCP must decide whether the primary identity represents:

- A single device endpoint.
- A user-controlled identity that can have multiple devices.
- A hierarchy containing a long-term identity and device-specific identities.

The first implementation should avoid silently conflating these models. Multi-device support may require additional key authorization and revocation procedures.

## 5. Identity creation

A conforming implementation MUST use cryptographically secure randomness from the host platform or an approved cryptographic library.

Identity creation MUST:

- Generate key material using the selected reviewed scheme.
- Avoid deterministic identifiers derived from public personal information.
- Avoid logging private key material.
- Store secrets only through a defined secure-storage strategy.
- Provide a clear failure path if secure randomness or required storage is unavailable.

The exact key types and generation procedures are open.

## 6. Identity verification

A secure session requires a defined method for authenticating peer identity material.

Possible verification mechanisms include out-of-band fingerprint comparison, QR-based verification, signed identity statements, or a trusted introduction mechanism. These are alternatives for evaluation, not decisions.

A server-provided key MUST NOT automatically be considered authentic solely because it was delivered by that server.

The protocol must specify how identity changes are detected and communicated.

## 7. Rotation, revocation, and recovery

The final identity specification MUST define:

- How identity keys are rotated.
- How old keys are retired.
- How compromised identities are revoked.
- How peers learn about revocation.
- What happens to existing sessions after rotation.
- Whether recovery is possible and what security trade-offs it introduces.

Recovery mechanisms MUST NOT silently weaken authentication or permit an infrastructure operator to impersonate an endpoint.

## 8. Identifier privacy

Stable identifiers can enable correlation across conversations, relays, and time.

Implementations SHOULD avoid exposing a single globally stable identifier to every relay or transport provider unless required by a documented protocol feature.

The privacy model must document which identifiers are visible to each participant.

## 9. Application boundary

Applications may map their own account IDs or contact records to PCP identities. That mapping is application-owned.

PCP MUST NOT require a public directory of all users or devices.

## 10. Open decisions

- Device identity versus user identity hierarchy.
- Public identity representation and canonical encoding.
- Peer verification and key transparency options.
- Rotation and revocation protocol.
- Multi-device authorization.
- Backup and recovery model.
- Whether unlinkable or pairwise identifiers are required for the first release.
