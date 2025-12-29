---
title: Compatibility Guideline
aliases:
  - /engine/compatibility-guideline
---

# Compatibility Guideline

*This file was written based on engine version 5.14.0-dev.*

When implementing features or extending the behavior of existing APIs, we generally want the following:

1. Backwards compatibility: Keep existing features working (according to [versioning](/for-engine-devs/version-number.md)).
2. Forwards compatibility and future-proofing: Make it as easy as possible to extend the feature in the future.

This document gives a few insights on how to achieve backwards and forwards
compatibility among:

* old server + new client
* new server + old client


## Imperfect backwards compatibility

In many cases it is not possible to implement a feature such that it works perfectly
well on older clients. A compromise has to be defined.

**Example**: Objects shall have a fade-in and fade-out animation, which is specified
by `float fade_time`, sent from the server to the client. However, older clients are
not aware of this parameter, thus would not play any fade-out animation.
Possible approaches:

* After `fade_time` seconds, the server sends a new packet to set `is_visible = false`
  for older clients
* The caller mod shall consider the protocol version and include a workaround if
  backwards compatibility is wanted by the author.


## Networking

*Note that the code examples below were taken from the codebase*
*and modified significantly for easier understanding.*

### Append to existing

This is useful when adding a few data bytes to existing packets.
For larger changes, consider using a [Protocol version check](#protocol-version-check) instead.

#### For `std::iostream`

For the sender (client or server) it is often simple to *append* new data to the
previous end of the stream. Let us assume that `shaded` shall be added:

```C++
void ObjectProperties::serialize(std::ostream &os) const
{
	// ... other data writes
	os << serializeString16(damage_texture_modifier);
	// << previous end >>

	// New data byte(s):
	writeU8(os, shaded);
	// << new end >>
}
```

Meanwhile the counterpart must detect and deal with differing stream lengths:

```C++
void ObjectProperties::deSerialize(std::istream &is)
{
	// ... other data reads
	try {
		// Note 1: String deserialize throws an exception if `is` ends too early.
		damage_texture_modifier = deSerializeString16(is);
	} catch (SerializationError &e) {
		return;
	}
	// << previous end >>

	// New data byte(s):
	if (!tryRead<bool, readBool>(shaded, is)) { // see 'Note 3' below
		// Note 2: `shaded` must be assigned to a fallback value. But not here.
		// Use a code path that is guaranteed to be executed, such as a constructor.
		return;
	}
	// << new end >>
}
```

Note 3: The `read*(std::istream &)` deserialize functions do **not** throw an exception
when the stream ends too early for the first time. Use `tryRead<T, reader_fn>` to
correctly detect End Of File.

#### For `NetworkPacket`

Same as with `std::iostream`, new bytes can be *appended* to the previous packet data.
A few known good examples:

 * `Server::SendPlayerFov` (write) and `Client::handleCommand_Fov` (read)
 * `Client::sendUpdateClientInfo` (write) and `Server::handleCommand_UpdateClientInfo` (read)

**Recommendations for reader functions**

* The `PacketError` exception. It is thrown reliably when attempting to read beyond
  the available data bytes.
   * This is demonstrated in the examples listed above.
* `NetworkPacket::getRemainingBytes` as an alternative to `PacketError` as seen in:
   * `Server::process_PlayerPos`
   * `Client::handleCommand_CloudParams`


### Protocol version check

*This section may also be applied to persistent data, e.g. mapblock data.*

For trivial additions, consider using [Append to existing](#append-to-existing) instead.
Reason for that being the *version bump*.

#### Handshake

The *protocol version* is a number resulting from the handshake between the
server and client. It is calculated as follows:
```
protocol_version = min(highest_supported_by_client, highest_supported_by_server)
```

* Client: `Client::m_proto_ver`
* Server: `RemotePlayer::protocol_version`

*Consequently, the value of these two variables is guaranteed to be equal.*

#### Code in practice

This means that the following logic can be used for both - client and server:

```C++
void X::SendFooBar(session_t peer_id)
{
	// NetworkPacket pkt(...)

	if (protocol_version < 50) {
		// Compatibility code used for older engine versions
	} else {
		// New code path
	}
}
```

**However**, this means that a new version number is needed ("*protocol version bump*").
Whereas the version is [bumped regularly](/for-engine-devs/releasing-luanti), it means that:

* Your change can only be tested by bumping the protocol version locally ahead of time ...
* ... or you have to increment `LATEST_PROTOCOL_VERSION` yourself.


A few known good examples:

* `Server::SendAddParticleSpawner` (write) and `Client::handleCommand_AddParticleSpawner` (read)
* `Server::SendItemDef` (write) and `Client::handleCommand_ItemDef` (read)


## Formspec

### Adding elements

Older clients silently ignore unknown elements.

In addition to checking the sanity of the formspec element (`parts.size()`) it is
desired to allow extending the element down the road. For this purpose, so use
`GUIFormSpecMenu::precheckElement` as demonstrated below.

```C++
	if (!precheckElement("bgcolor", element, 1, 3, parts))
		return;
```

### Appending parameters

*This section assumes that `GUIFormSpecMenu::precheckElement` is being used.*

Older clients will silently ignore appended parameters **if** the version specified by
`formspec_version[N]` is *newer* than what the client supports. For example, a
hypothetical element called `mylabel` can be extended by using a more recent
*formspec version*.

Mod code **before**:

```Lua
	local formspec =
		"formspec_version[15]"
		"size[10,9]" ..
		"mylabel[2,5;1,1;Hello World]"
```

Mod code **after**:

```Lua
	local formspec =
		"formspec_version[16]"
		"size[10,9]" ..
		"mylabel[2,5;1,1;Hello World;#FF0000]"
```

Meanwhile, the C++ code needs the following adjustment:

```C++
	// Previous code line:
	if (!precheckElement("mylabel", element, 3, 3, parts))
		return;
	// Adjusted line to allow one additional argument on new clients:
	if (!precheckElement("mylabel", element, 3, 4, parts))
		return;
```

As well as incrementing *and documenting* the new `FORMSPEC_API_VERSION`.

### Changing parameters

Example: A parameter shall not only accept boolean values but numeric too.

This is only possible in rare cases. Compatibility with older engine versions should
be tested thoroughly.
