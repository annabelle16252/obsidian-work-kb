# Layer 4

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Layer 4
# QUIC #
https://juniper.lightning.force.com/lightning/r/Knowledge__kav/ka0Dp000000ip06IAA/view
Overview of HTTP/3 and QUIC:... ...HTTP/3 is the upcoming third major version of the HTTP protocol, which utilizes QUIC instead of TCP. QUIC is a new transport protocol based on UDP, designed to offer improved performance.
Key Differences and Advantages:
Efficiency:... ...Traditional HTTPS with TLS 1.2 requires 1 RTT (Round-Trip Time) for the TCP handshake and an additional 2 RTTs for the TLS handshake. In contrast, HTTP/3 with QUIC combines the TLS handshake with the QUIC handshake, requiring only 1 RTT. Moreover, if two endpoints have previously established a connection, they can use cached data to set up a 0-RTT connection, further reducing latency.
Address Independence:... ...QUIC is designed to be address-independent. Instead of using an address/port tuple, a QUIC endpoint uses a connection ID to match packets to connections. This allows a QUIC endpoint to change its address or port without closing the connection.
Encryption:... ...QUIC encrypts data at the packet level, ensuring confidentiality and integrity. Both the packet header and payload are encrypted independently, which enhances security.
HTTP/2 over TCP = A train with many cars: if one car derails (packet loss), the whole train stops.
HTTP/3 over QUIC (UDP) = Many small cars driving independently: one delay doesn’t stop the others.
Simple Analogy
Limitation with MX Captive Portal Redirection:... Since the CPCD daemon does not understand the QUIC protocol, the MX series routers do not support QUIC for captive portal redirection. This limitation arises because the encryption in QUIC prevents the CPCD daemon from processing or redirecting the traffic.
