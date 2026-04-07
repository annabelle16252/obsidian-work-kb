# GRE

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

GRE
# ...FTI... #
FTI is a pseudo non anchored interface that is able to carry different types of payloads (IPv4,IPv6 and ISO) over IPv4 or IPv6 transport network using different tunnel encapsulations (GRE,IPIP and UDP). FTI interface can support inet,inet6 and ISO families for ENCAP and inet.inet6,iso and mpls for DECAP.
FTI tunnels support two encapsulation modes:
1.    Loopback encapsulation mode: default mode
This is two pass encapsulation and packet destined to go through the FTI tunnel takes two passes before it’s sent out on egress interface in tunnel ingress node .packet has to enter ingress pipeline back via PFE internal loopback interface which has limited BW of 400G and shared with multicast application.
2.    Flattened encapsulation mode: If bypass-loopback is configured.
This is Single pass encapsulation and packet destined to go through the FTI tunnel takes only single pass before it’s sent out on egress interface in tunnel ingress node. whole PFE's BW can be used for FTI
bypass-loopback;  // Optional        <<<<<<<<without this know , BW is limited to 400G
KB75432... ...[PTX10008] GRE Tunnel using flexible tunnel interface
