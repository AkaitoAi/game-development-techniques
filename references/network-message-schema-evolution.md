# Network message schema evolution

## Problem and design reasoning

Two builds can receive the same bytes and assign different meanings to them. Even a message that parses can produce incorrect gameplay when defaults, units, or enum meanings change. The released *Quake* client checks a server protocol version before parsing the remainder of its server info. The *Tunnel Town* team described using Protocol Buffers for serialized state sent across its client-server boundary to support online, offline, and multi-device play while limiting server logic. The compatibility policy below is a design inference. A tightly encoded match protocol can deliberately require a shared version, while messages that must survive mixed builds benefit from explicit compatibility rules. [id Software, Quake client source](https://github.com/id-Software/Quake/blob/master/WinQuake/cl_parse.c) · [Geurts and Haycock, Tunnel Town developer slides, GDC Next 2013](https://media.gdcvault.com/gdcnext2013/slides/Geurts%26Haycock.pdf)

## Techniques and practical checks

### Choose compatibility per message family

List the peer pairs that must interoperate and the duration of support. Negotiate a protocol revision for messages with tightly coupled simulation semantics. Use explicit feature capabilities where a client can participate safely with a reduced feature set. Stop before gameplay begins if the peers cannot agree on required behavior.

**Check** connect current and previous test clients to supported servers. Confirm a shared feature set for compatible peers and a clear upgrade result for unsupported peers.

### Evolve fields without reinterpreting old data

Keep field identities, units, and meaning stable. Give an optional field a documented absence behavior and reserve retired tags in formats that use numbered fields. For an incompatible semantic change, introduce a new field or message revision. Follow the actual serializer rules for unknown fields, enum values, and relays. [Protocol Buffers language guide](https://protobuf.dev/programming-guides/proto3/)

**Check** decode old and new fixtures in both directions. Include omitted fields, meaningful zero values, deprecated values, and an unknown enum. Compare the interpreted gameplay values as well as parse results.

### Convert wire data at the boundary

Map supported message versions to a canonical gameplay command or snapshot. Perform bounds and domain validation before applying it. Give absent data a deliberate interpretation and group related modifications so decoding failure cannot leave partially applied state.

**Check** send fixtures for valid old, valid new, and incomplete application messages to a local test endpoint. Verify equivalent intentions produce equivalent domain actions and rejected inputs leave gameplay state intact.

### Retain representative compatibility fixtures

Keep encoded messages and expected interpretations for supported versions alongside high-level client-server tests. Include a delta's baseline identity, when applicable, so old baselines cannot be decoded under a changed schema. Record version and rejection categories instead of retaining unnecessary player payloads.

**Check** rerun fixtures after a format edit and test reconnects around a negotiated version or capability change. Confirm a fresh compatible baseline is established and measure the size of representative messages against the network budget.
