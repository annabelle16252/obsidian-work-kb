# HA

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

HA
GRES: ...preserve interface and kernel information
NSR: ...synchronizes routing protocol information by running the r...pd... on the backup R...E
GR: need peer device as helper
# GRES #
> ...request chassis routing-engine master switch check
> ...show system switchover...  <<< on backup RE
Routing platform processes that are not part of GRES (such as the routing protocol process (rpd)) restart.... Therefore, to preserve routing during a switchover, GRES must be combined with either GR or NSR.
# NSR #
# ...set routing-options nonstop-routing
