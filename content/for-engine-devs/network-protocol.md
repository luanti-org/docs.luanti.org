---
title: Network Protocol
aliases:
  - /Engine/Network_Protocol
  - /engine/network-protocol
---

# Engine/Network Protocol

## High-level protocol

The high-level protocol is clearly written down and updated in [networkprotocol.h](https://github.com/luanti-org/luanti/blob/master/src/network/networkprotocol.h).

For specifics, refer to [client.cpp](https://github.com/luanti-org/luanti/blob/master/src/client/client.cpp) and [server.cpp](https://github.com/luanti-org/luanti/blob/master/src/server.cpp) for the sending, and [clientpackethandler.cpp](https://github.com/luanti-org/luanti/blob/master/src/network/clientpackethandler.cpp) and [serverpackethandler.cpp](https://github.com/luanti-org/luanti/blob/master/src/network/serverpackethandler.cpp) for the receiving.

There is also an ascii art inside [clientiface.h](https://github.com/luanti-org/luanti/blob/master/src/server/clientiface.h) describing the protocol from a high level perspective.

Actual minimum protocol version is: **24**. This corresponds to version **0.4.11**.

### Handshake

From client's standpoint:

- \-> TOSERVER_INIT
- <- TOCLIENT_HELLO
- <-> Authentication
- <- TOCLIENT_AUTH_ACCEPT
- \-> TOSERVER_INIT2
- <- Many many things

If you just want to check if a server is alive, you can disconnect after receiving TOCLIENT_INIT. Note that handshake changed with protocol 25. In order to be compliant with both old and new servers, send both TOCLIENT_INIT and TOCLIENT_INIT_LEGACY with a protocol version >= 25. New servers will ignore TOCLIENT_INIT_LEGACY that have a protocol version >= 25, and listen for TOCLIENT_INIT. Old servers will ignore TOCLIENT_INIT because its an unknown packet for them.

The handshake before protocol 25 was, from client's standpoint:

- \-> TOSERVER_INIT_LEGACY
- <- TOCLIENT_INIT_LEGACY
- \-> TOSERVER_INIT2
- <- Many many things

### Authentication

Starting with protocol 25, Luanti got more generic authentication handling.

#### Authentication before protocol 25 (<=0.4.12)

The client sent the desired username and a hash of the password salted with the username to the server inside TOSERVER_INIT_LEGACY (see the translatePassword function inside util/string.h for details on the hash). The server then compared the passed hash with an entry from the auth backend, and sent TOCLIENT_INIT_LEGACY on success, TOCLIENT_ACCESS_DENIED_LEGACY when authentication failed. On first login, the server already had the value to store inside its database. Similarly, the "change password" functionality was done using the TOSERVER_PASSWORD_LEGACY packet, where the client sent hash of old and desired new packet, and the server updated the value.

#### Authentication since protocol 25

Authentication is now handled with different "authentication mechanisms". In the the TOSERVER_INIT packet, the client sends the desired username. Then the server replies with a list of supported authentication mechanisms in TOCLIENT_HELLO. The client then choses a supported auth mechanism, and starts the auth. Currently there are three auth mechanisms in usage, all around the [Secure Remote Password Protocol (SRP)](https://en.wikipedia.org/wiki/Secure_Remote_Password_protocol):

1\. AUTH_MECHANISM_SRP: Modern srp salted with the server provided salt. This auth mechanism is presented by the server if it has a modern srp verification key.

2\. AUTH_MECHANISM_LEGACY_PASSWORD: srp with the legacy database hash used as "salt". The server presents this auth mechanism if the database only contains the legacy hash.

3\. AUTH_MECHANISM_FIRST_SRP: First login, setting (srp) password, or to change the (srp) password.

The actual SRP exchange is done in accordance with [RFC 2945](https://tools.ietf.org/html/rfc2945), using [RFC5054's 2048 group](https://tools.ietf.org/html/rfc5054#appendix-A), and SHA-256 as hash algorithm. The only difference from RFC 2945 is that user names are lowercased by default, in order to make the login protocol case insensitive.

##### FAQ design choices

- **Why two srp auth methods? Why not just one, that's also working with the legacy hash?**: Its correct, that both LEGACY and modern login methods allow to prevent replay attacks, and give men in the middle no way to offline brute-force the password, serving the two main srp advantages. However, if you look at the differences between the modern and the legacy auth mechanisms, you will notice that the new method has case insensitivity baked in.
- **Why these needless 'auth mechanism' abstractions? You are using three different types of auth mechanism support (login, sudo mode login, setting password from sudo mode)**: The abstractions are necessary for future extensibility. Also they make sense, as even for setting up passwords you will want to make authentication fail-able. Think of additional e-mail verification steps at account setup, but we are using this already inside current protocol, when the server checks the is_empty value sent by the client to match with non empty password policies. And third, the abstractions come at very low cost, check how lightweight it is by having a look at the "setting password from sudo mode".

## Low-level protocol

References: [connection.h](https://github.com/luanti-org/luanti/blob/master/src/network/connection.h) [connection.cpp](https://github.com/luanti-org/luanti/blob/master/src/network/connection.cpp)

The Luanti protocol is a small layer on top of UDP. There is a header and four packet types.

All numbers are big-endian.

```
A packet is sent through a channel to a peer with a basic header:
    Header (7 bytes):
    [0] u32 protocol_id
    [4] u16 sender_peer_id
    [6] u8 channel
sender_peer_id:
    Unique to each peer.
    value 0 is reserved for making new connections
    value 1 is reserved for server
channel:
    The lower the number, the higher the priority is.
    Only channels 0, 1 and 2 exist.
*/
#define BASE_HEADER_SIZE 7
#define PEER_ID_INEXISTENT 0
#define PEER_ID_SERVER 1
#define CHANNEL_COUNT 3

```

**Channels aren't really used much.** The channel priority thing hasn't actually ever been implemented and probably never will. Also, channels are intended to provide a few parallel reliable streams when needed - thus if one sends big reliable chunks of data in one channel and then small reliable packets in an another, and one of the pieces of the large chunk gets dropped and has to be re-sent, the small packets will see no delay. Because the order between channels is not maintained, care must be taken to not send stuff in different channels that needs to arrive in the right order.

```
/*
Packet types:

CONTROL: This is a packet used by the protocol.
- When this is processed, nothing is handed to the user.
    Header (2 byte):
    [0] u8 type
    [1] u8 controltype
controltype and data description:
    CONTROLTYPE_ACK
        [2] u16 seqnum
    CONTROLTYPE_SET_PEER_ID
        [2] u16 peer_id_new
    CONTROLTYPE_PING
    - There is no actual reply, but this can be sent in a reliable
      packet to get a reply
    CONTROLTYPE_DISCO
*/
#define TYPE_CONTROL 0
#define CONTROLTYPE_ACK 0
#define CONTROLTYPE_SET_PEER_ID 1
#define CONTROLTYPE_PING 2
#define CONTROLTYPE_DISCO 3

/*
ORIGINAL: This is a plain packet with no control and no error
checking at all.
- When this is processed, it is directly handed to the user.
    Header (1 byte):
    [0] u8 type
*/
#define TYPE_ORIGINAL 1
#define ORIGINAL_HEADER_SIZE 1

/*
SPLIT: These are sequences of packets forming one bigger piece of
data.
- When processed and all the packet_nums 0...packet_count-1 are
  present (this should be buffered), the resulting data shall be
  directly handed to the user.
- If the data fails to come up in a reasonable time, the buffer shall
  be silently discarded.
- These can be sent as-is or atop of a RELIABLE packet stream.
    Header (7 bytes):
    [0] u8 type
    [1] u16 seqnum
    [3] u16 chunk_count
    [5] u16 chunk_num
*/
#define TYPE_SPLIT 2

/*
RELIABLE: Delivery of all RELIABLE packets shall be forced by ACKs,
and they shall be delivered in the same order as sent. This is done
with a buffer in the receiving and transmitting end.
- When this is processed, the contents of each packet is recursively
  processed as packets.
    Header (3 bytes):
    [0] u8 type
    [1] u16 seqnum

*/
#define TYPE_RELIABLE 3
#define RELIABLE_HEADER_SIZE 3

#define SEQNUM_INITIAL 65500

```

These packet types are used in practice:

- CONTROL(data) - unreliable control packet
- ORIGINAL(data) - unreliable small data
- SPLIT(piece of data) - unreliable piece of large data
- RELIABLE(CONTROL(data)) - reliable control packet
- RELIABLE(ORIGINAL(data)) - reliable small data
- RELIABLE(SPLIT(piece of data)) - reliable piece of large data

### Packet Type Specifics

#### RELIABLE

The RELIABLE packet wraps any other packet inside it, and once it is received, an ACK/CONTROL is sent back. If ACK/CONTROL isn't received for a sent RELIABLE packet in a short time, it should be periodically re-sent until the related ACK/CONTROL is received.

The sequence number is per-peer-per-connection. It is incremented after every reliable packet that is sent. It is not incremented in re-sent packets.

#### SPLIT

The SPLIT packet means data is split into multiple packets (because the internet doesn't transfer UDP packets larger than ~500 bytes); once all pieces of a SPLIT packet are received (identified as having the same seqnum and accumulating all the pieces 0...chunk_count-1), the insides is returned to the user of the network stack like data of an ORIGINAL packet.

### Low-level control

#### Timeout

A connection may timeout after 30 seconds of non-responsiveness to PINGs or RELIABLEs. A peer should send PING/CONTROL packets every 5 seconds or so if it has not sent any other packets.

#### Connect

A connection is initiated by sending an empty RELIABLE(ORIGINAL()) packet. The server will reply with a SET_PEER_ID/CONTROL packet. The client's peer id from there on is the one received from the server and the client can continue communicating by using it.

#### Disconnect

A connection is properly disconnected by sending a DISCO/CONTROL packet before dropping a connection.
