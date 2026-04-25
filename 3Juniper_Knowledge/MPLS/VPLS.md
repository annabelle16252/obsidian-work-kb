# VPLS

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

VPLS
# VPLS Types #
-BGP Based:
Use NLRI
-A VT interfaceis created for each remote site.
Traffic from core are passed through VT interface so that they can be forwarded based on MAC add.
-LDP Based:
Also has bgp auto-discovering, FEC129
-VPLS pacekt flow
Vpn-name.vpls==> this is MAC table (FIB)
-BUM ...（...bgp mpls only...）
# VLAN Normalization #
<<< vlan id ...只在...local site...有用，为了学...mac...的。...local...和...remote site...之间...vlan...不同，不影响...vpls...的互通。
<<< We usually advise to configure all PEs with same VPLS vlan-id. If not, routing interface(l3 interface) will not be able to communicate to the remote CE.
