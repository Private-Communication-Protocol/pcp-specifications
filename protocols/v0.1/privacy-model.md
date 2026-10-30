# PCP Privacy and Metadata Model

**Status:** Draft  
**Version:** 0.1.0-draft

## 1. Purpose

This document defines PCP's privacy objectives and requires explicit documentation of metadata visible to applications, endpoints, relays, and network observers.

End-to-end encryption protects message content. It does not automatically hide who communicates, when they communicate, how much data they exchange, or which infrastructure they use.

## 2. Privacy goals

PCP aims to:

- Minimize unnecessary metadata collection and disclosure.
- Avoid mandatory global user and device directories.
- Avoid requiring a single central service.
- Keep message content opaque to relays.
- Support privacy-preserving transport integrations.
- Make unavoidable metadata exposure explicit.
- Avoid claiming stronger anonymity than the selected architecture can provide.

## 3. Data categories

The design must distinguish:

### 3.1 Content

Application payloads, attachments, and application-defined data. These should be protected end to end.

### 3.2 Protocol metadata

Information needed to process a message, such as protocol version, message class, session references, counters, or cryptographic headers. Some of this may be visible depending on the selected construction.

### 3.3 Routing metadata

Information needed to move data, such as relay addresses, mailbox identifiers, routing tokens, or destination references.

### 3.4 Network metadata

Source and destination IP addresses, DNS queries, connection timing, duration, packet sizes, and traffic volume.

### 3.5 Application metadata

Account identifiers, contact relationships, group membership, presence, read receipts, typing indicators, and user profiles. These remain application-defined unless a future PCP extension explicitly specifies otherwise.

## 4. Observer visibility

The final specification must provide a visibility matrix for each supported deployment.

At minimum, evaluate what each party can observe:

| Observer | Questions to answer |
|---|---|
| Sending application | It normally sees the plaintext it submits and its own application state. |
| Sending endpoint | Which peer, session, and transport details are locally available? |
| Network access provider | Which addresses, timing, and traffic volume are visible? |
| Relay operator | Which client address, routing token, timing, and message size are visible? |
| Receiving endpoint | Which sender/session information is authenticated and available? |
| Receiving application | Which plaintext and application metadata does it receive? |
| Colluding observers | Can observations at multiple points be correlated? |

Do not fill this matrix with assumptions. Complete it for each actual transport and delivery design.

## 5. Stable identifiers

Stable identifiers can enable long-term correlation.

The identity and routing specifications SHOULD minimize global identifiers and evaluate pairwise or short-lived identifiers where practical.

Any stable identifier exposed to a relay must have a documented purpose and privacy trade-off.

## 6. Traffic analysis and anonymity

Low-latency onion routing can reduce direct exposure of a client's network address to a destination or relay, depending on the path and threat model. It does not guarantee protection against a global observer who can correlate timing and traffic volume at multiple points.

Mix networks, batching, delays, and cover traffic may offer different traffic-analysis properties but introduce latency, complexity, and resource costs.

PCP must describe these trade-offs rather than treating encryption or onion routing as synonymous with anonymity.

## 7. Data minimization

Implementations and infrastructure SHOULD:

- Collect only metadata required for defined protocol functions.
- Avoid unnecessary persistent logs.
- Avoid global presence or device directories.
- Avoid stable cross-context identifiers where not required.
- Define retention and deletion behavior for temporary delivery data.
- Keep diagnostics free of plaintext and secrets.
- Document telemetry explicitly; telemetry should not be silently introduced by the protocol.

## 8. Application boundary

PCP does not decide whether an application uses usernames, contacts, groups, presence, receipts, or typing indicators.

If an application transports such events through PCP, the application remains responsible for their semantics and policy. Their metadata exposure must still be considered.

## 9. Privacy claims

Every public privacy claim should identify:

- The adversary being considered.
- The transport and deployment assumptions.
- Which metadata is protected.
- Which metadata remains observable.
- Whether collusion or global observation is in scope.
- Known limitations and residual risks.

Absolute anonymity MUST NOT be claimed.

## 10. Open decisions

- Metadata visibility of the selected envelope.
- Pairwise or rotating routing identifiers.
- Relay-side logging and retention requirements.
- Padding and size-hiding strategy.
- Batching, delays, or cover traffic.
- Supported anonymity transports and threat assumptions.
- Privacy testing and measurement methodology.
