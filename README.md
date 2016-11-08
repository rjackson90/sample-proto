# Simple Message Protocol

This project is meant to only take a few evenings to complete, and is heavily inspired by 
other projects I have done recently using the new [Tokio](https://github.com/tokio-rs/tokio) 
family of crates. It's primary purpose is to be a code sample, with a secondary purpose 
of building expertise creating fast, reliable, asynchronous servers. 

## License

This project is licensed under GPLv3, in order to preserve any future contributions for 
others to learn from. Plus, the primary purpose of this project is to enhance learning, 
not to be useful to commercial projects. 

## Protocol
The actual protocol is going to be a hybrid of a trivial line-based protocol, and the 
[WebSocket](https://tools.ietf.org/html/rfc6455) protocol, in that there will be a (simple)
handshake, and support for text, binary, and a limited set of control messages. The wire
format and base framing protocol use the WebSocket equivalent as a base, with a couple of
simplifying features. 

### Data Framing Protocol
Please see [Section 5.2 of RFC6455](https://tools.ietf.org/html/rfc6455#section-5.2) for a reference
of the starting point for this design. Designing an entire network protocol is outside the
scope of what I'm trying to accomplish, so I'll start there and make the following changes:
  - Bits 1-7 are opcode bits, no more RSV bits
  - Bits 8-32 are the payload length. Max payload length is (2^25-1), full stop. 
  - Payload data follows immediately after the length field. Unlike WebSockets, no messages are masked. 
