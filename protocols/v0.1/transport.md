# PCP Transport Abstraction

**Status:** Draft  
**Version:** 0.1.0-draft

## 1. Purpose

This document defines requirements for the network transport layer used by PCP.

Transport moves protocol data between endpoints and optional infrastructure. It is not the source of end-to-end message confidentiality.

## 2. Design principles

The transport architecture SHOULD:

- Be modular and replaceable.
- Avoid binding the protocol to one network or proxy.
- Support independently operated infrastructure.
- Minimize transport metadata where practical.
- Keep application payloads encrypted end to end.
- Define failure, timeout, and reconnection behavior.
- Make transport-specific privacy guarantees explicit.

## 3. Transport interface

A transport adapter is expected to provide abstract operations for:

- Establishing a connection or route.
- Sending opaque protocol data.
- Receiving opaque protocol data.
- Reporting connection and delivery errors.
- Closing or resetting a connection.
- Applying documented limits and backpressure.

The exact API is implementation-specific unless a future specification defines a cross-language interface.

## 4. Candidate transport classes

PCP may evaluate:

- Direct client-to-client connections.
- Conventional client-to-server relays.
- Onion-routed transports.
- Store-and-forward delivery services.
- Other independently operated network paths.

These are candidate classes, not a commitment to support all of them in v0.1.

## 5. Transport security versus end-to-end security

Transport encryption, such as TLS, protects a network connection to the transport peer. It does not replace end-to-end encryption between the communicating endpoints.

A relay or proxy MUST NOT be trusted with plaintext application payloads merely because the connection to that relay is encrypted.

## 6. DNS and routing privacy

Transport adapters must document where name resolution occurs and which parties can observe destination information.

Where a privacy transport supports remote name resolution, implementations SHOULD avoid local DNS resolution that would reveal destinations outside the intended privacy path.

The exact behavior depends on the selected transport and must be tested.

## 7. Connection lifecycle

The transport specification must define:

- Connection establishment and authentication requirements, if any.
- Timeouts and retry behavior.
- Reconnection behavior.
- Backpressure and queue limits.
- Handling of partial writes and interrupted reads.
- Maximum frame or record sizes.
- Resource limits against denial of service.

## 8. Transport metadata

Transport providers may observe some combination of source address, destination, timing, message size, connection duration, and traffic volume.

The privacy model must describe this visibility for each supported transport.

## 9. Failure and fallback

Implementations MUST NOT silently downgrade to a transport that violates the application's configured security or privacy requirements.

If a required privacy transport is unavailable, the implementation should report failure rather than quietly bypass it.

## 10. Open decisions

- First supported transport.
- Transport framing and stream semantics.
- Connection authentication.
- Relay routing and discovery.
- DNS behavior.
- Retry and backpressure rules.
- Proxy configuration interface.
- Transport capability negotiation.
